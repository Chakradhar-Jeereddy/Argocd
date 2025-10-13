## Diffing customization
Basically informing ArgoCD to ignore difference during autosync
- Ignore differences of problematic resources/manifests
- Diffing can be configured at application level or system level.

## Options to implement
JSON path
JQ path
fields

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: diffing-customization-demo
  namespace: argocd
spec:
  destination:
    namespace: diffing-customization-demo
    server: "https://kubernetes.default.svc"
  project: default
  source:
    path: guestbook-with-sub-directories
    repoURL: "https://github.com/Chakradhar-Jeereddy/argocd-example-apps.git"
    targetRevision: master
    directory:
      recurse: true
  syncPolicy:
    automated: {}
    syncOptions:
      - CreateNamespace=true
  ignoreDifferences:
    - group: apps
      kind: Deployment
      jsonPointers:
      - /spec/replicas
```
- Autosync will deploy resources, thereafter increase replicas in deployment manifest in git and commit.
- Autosync will not increase replicas, it will ignore the differences
- The forcfully accept the differences do a manual sync.
