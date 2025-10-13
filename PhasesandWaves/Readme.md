### Sync phases and Hooks and waves.
### To define logic or an order or specific sequence of execution.
- Hooks are kubernetes temporary resource manifests (like, jobs, pods and so on)
- These are mainly user for controlled execution of tasks and resource manifest.
- Control sync flow (pause, approve, clean up)
- Integrate with CI/CD pipelines.

**Sync phases (default is sync)***
- presync
- sync
- postsync

| Hook Type    | When it runs                                    | Typical use                                     |
| ------------ | ----------------------------------------------- | ----------------------------------------------- |
| **PreSync**  | Before any manifests are applied                | Pre-deployment checks, DB migrations            |
| **Sync**     | During manifest apply (instead of normal apply) | Custom deployment logic, overrides default sync |
| **PostSync** | After all manifests are applied successfully    | Validation, smoke tests, notify systems         |
| **SyncFail** | If sync fails                                   | Cleanup, rollback, send alerts                  |
| **SyncWave** | Controls order of resource creation             | CRDs before CRs, backend before frontend        |


***Control which resource manifests should be deployed in presync or sync or else postsync.
These phases are defined at resource manifest level using hook annotations.
Argocd goes to next phase only when previous phase is succeded and healthy.
These hooks doesn't run during selective sync.***
```
Application
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata: 
  name: sync-phases
  namespace: argocd
spec:
  destination:
    namespace: sync-phases
    server: "https://kubernetes.default.svc"
  project: default
  source: 
    path: sync-phases
    repoURL: "https://github.com/mabusaa/argocd-example-apps.git"
    targetRevision: master
  syncPolicy:
    syncOptions:
      - CreateNamespace=true
```

```
Manifest
apiVersion: batch/v1
kind: Job
metadata:
  name: before
  annotations:
    argocd.argoproj.io/hook: PreSync                
    argocd.argoproj.io/hook-delete-policy: HookSucceeded
spec:
  template:
    spec:
      containers:
      - name: sleep
        image: alpine:latest
        command: ["sleep", "10"]
      restartPolicy: Never
  backoffLimit: 0
```

### When Argocd deletes the hook resources (like jobs, pods) after they run.
- Each hook (PreSync, PostSync, SyncFail, etc.) is a Kubernetes resource — usually a Job, Pod, or something temporary.
- By default, Argo CD doesn’t automatically delete these after they finish, which could clutter your cluster over time.
- That’s where the annotation argocd.argoproj.io/hook-delete-policy

| Value                                       | Meaning                                                     | When deletion happens                         |
| ------------------------------------------- | ----------------------------------------------------------- | --------------------------------------------- |
| **BeforeHookCreation**                      | Delete previous hook resource **before creating** a new one | Prevents duplicates if re-syncing             |
| **HookSucceeded**                           | Delete hook resource after it **completes successfully**    | Keeps only failed hooks for debugging         |
| **HookFailed**                              | Delete hook resource after it **fails**                     | Keeps only successful hooks                   |
| **Always** (not official, but combo of all) | You can specify multiple policies separated by commas       | `BeforeHookCreation,HookSucceeded,HookFailed` |


***Cleanup after success***
```
metadata:
  annotations:
    argocd.argoproj.io/hook: PostSync
    argocd.argoproj.io/hook-delete-policy: HookSucceeded
```
***Keep failed jobs for debugging
```
metadata:
  annotations:
    argocd.argoproj.io/hook: PreSync
    argocd.argoproj.io/hook-delete-policy: HookSucceeded
```
***Only successful hooks are deleted — failed ones remain for inspection.***
```
metadata:
  annotations:
    argocd.argoproj.io/hook: SyncFail
    argocd.argoproj.io/hook-delete-policy: BeforeHookCreation,HookSucceeded,HookFailed
```
***Delete all hooks always***

### Sync waves
- Define order of deployment in each sync phase
- Waves starts from zero or lower number.
- It starts with next wave only when previous wave is health.
  
