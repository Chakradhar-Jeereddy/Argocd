### If an appset or application is lost
- We can get it using kubectl commands since all these objects are stored in k8s.
```
kubectl get applicationset guestbook-2 -o yaml -n argocd

apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  creationTimestamp: "2025-10-13T13:54:56Z"
  generation: 1
  name: guestbook-2
  namespace: argocd
  resourceVersion: "379998"
  uid: 5dd0becf-889a-4091-8437-c9f2a792aa8e
spec:
  generators:
  - clusters:
      selector:
        matchLabels:
          non-prod: "true"
      template:
        metadata: {}
        spec:
          destination: {}
          project: ""
  template:
    metadata:
      name: '{{name}}-guestbook-2'
      namespace: argocd
    spec:
      destination:
        namespace: guestbook-2
        server: '{{server}}'
      project: default
      source:
        path: guestbook
        repoURL: https://github.com/mabusaa/argocd-example-apps.git
        targetRevision: master
      syncPolicy:
        syncOptions:
        - CreateNamespace=true
```
kubectl get application guestbook-2 -o yaml -n argocd
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  creationTimestamp: "2025-10-13T14:04:23Z"
  finalizers:
  - resources-finalizer.argocd.argoproj.io
  generation: 18
  name: local-cluster-guestbook-2
  namespace: argocd
  ownerReferences:
  - apiVersion: argoproj.io/v1alpha1
    blockOwnerDeletion: true
    controller: true
    kind: ApplicationSet
    name: guestbook-2
    uid: 5dd0becf-889a-4091-8437-c9f2a792aa8e
  resourceVersion: "381691"
  uid: cab08935-5022-4e92-986b-88aa4a0e2c69
spec:
  destination:
    namespace: guestbook-2
    server: https://kubernetes.default.svc
  project: default
  source:
    path: guestbook
    repoURL: https://github.com/mabusaa/argocd-example-apps.git
    targetRevision: master
  syncPolicy:
    syncOptions:
    - CreateNamespace=true
```









