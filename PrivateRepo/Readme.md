A private repo must be resgistered first with argocd before using it as source in application
Be it git or helm

The registration will create a secret object in kubernetes.
The repo registration can be done using ArgoCD CLI, UI and kubectl.

We can use SSH private key, username/password, username and token, certificate and private key.

```

apiVersion: v1
kind: Secret
metadata:
  name: private-repo-https
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: https://github.com/mabusaa/argocd-example-apps-private.git
  password: # password goes here, NOTE: dont push secrets into git, use sealed secrets as a solution for secrets in gitops.
  username: my-token
```
