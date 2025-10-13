## Project
- Logical grouping of applications
- Restricting sources
- Restricting destination (cluster and namespaces)
- Restricting cluster resources
- Restricting namespace resources
- Project roles to grant permissions (get, create, update, delete, sync, override) and generate token jwt

## Creating basic project
```
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: demo-project
  namespace: argocd
spec:
  description: Demo Project
  sourceRepos:
  - '*'

  destinations:
  - namespace: '*'
    server: '*'

  clusterResourceWhitelist:
  - group: '*'
    kind: '*'

  namespaceResourceWhitelist:
  - group: '*'
    kind: '*'
```
