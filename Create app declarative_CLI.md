## Create application declaratively

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata: 
  name: guestbook
  namespace: argocd
spec: 
  destination: 
    namespace: guestbook
    server: "https://kubernetes.default.svc"
  project: default
  source: 
    path: guestbook
    repoURL: "https://github.com/mabusaa/argocd-example-apps.git"
    targetRevision: master
  syncPolicy:
    syncOptions:
      - CreateNamespace=true
```

## Create app using the above YAML
```
kubectl apply -f application.yaml
argocd app list

kubectl apply -f application.yaml
application.argoproj.io/guestbook created

.\argocd app list
NAME              CLUSTER                         NAMESPACE  PROJECT  STATUS     HEALTH   SYNCPOLICY  CONDITIONS  REPO                          PATH       TARGET
argocd/guestbook  https://kubernetes.default.svc  guestbook  default  OutOfSync  Missing  Manual      <none>      https://github.com/mabusaa..  guestbook  master
```
## Sync the application from UI (By default it kept out of sync)
```
 .\argocd app list
NAME              CLUSTER                         NAMESPACE  PROJECT  STATUS  HEALTH   SYNCPOLICY  CONDITIONS  REPO                          PATH       TARGET
argocd/guestbook  https://kubernetes.default.svc  guestbook  default  Synced  Healthy  Manual      <none>      https://github.com/mabusaa/.. guestbook  master

kubectl get application
NAME        SYNC STATUS   HEALTH STATUS
guestbook   Synced        Healthy
```

## Creating appliction using cli
```
 .\argocd app create app-2 --repo https://github.com/mabusaa/argocd-example-apps.git --revision master --path guestbook
--dest-server https://kubernetes.default.svc --dest-namespace app-2 --sync-option CreateNamespace=true
application 'app-2' created
```
