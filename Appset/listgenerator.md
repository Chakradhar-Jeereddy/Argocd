## login to orgocd
```
 ./argocd login localhost:8080
WARNING: server certificate had error: tls: failed to verify certificate: x509: certificate signed by unknown authority. Proceed insecurely (y/n)? y
Username: admin
Password:
'admin:login' logged in successfully
Context 'localhost:8080' updated

```
```
## Appset
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: guestbook
  namespace: argocd                     #appset is deployed in agocd namespace with name guestbook
spec:
  generators:
  - list:
      elements:
      - cluster: local-dev
        url: https://kubernetes.default.svc
  template:
    metadata:
      name: '{{cluster}}-guestbook'
      namespace: argocd                # application object is deployed in orgocd namespace with name cluster-guestbook
    spec:
      project: default
      source:
        repoURL: https://github.com/mabusaa/argocd-example-apps.git
        targetRevision: master
        path: guestbook
      destination:
        server: '{{url}}'
        namespace: guestbook          # rources are created under guestbook namespace inside the k8s cluster
      syncPolicy:
         syncOptions:
          - CreateNamespace=true
```

### Command
```
$ ./argocd appset create appset.yaml
ApplicationSet 'guestbook' created
Name:               argocd/guestbook
Project:            default
Server:             {{url}}
Namespace:          guestbook
Source:
- Repo:             https://github.com/mabusaa/argocd-example-apps.git
  Target:           master
  Path:             guestbook
SyncPolicy:         <none>

```
