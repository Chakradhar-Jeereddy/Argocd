## Git Directory generator
- Deploy multiple applications into local cluster.
- Application name and namespace should be the same as base path name thats in git.
- Use one ApplicationSet manifest with git directory generator to achieve this.
- Deploy all helm charts under path (helmcharts/security-policy-charts).

## Only generator type cluster can acces labels from secret
## list, git directory generators doesn't require secret and can't access the lables from secret
  https://github.com/mabusaa/argocd-course-apps-definitions/blob/main/application%20set/git-directory-generator.yaml

```
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: security-apps
  namespace: argocd
spec:
  generators:
  - git:
      repoURL: https://github.com/mabusaa/argocd-example-apps.git
      revision: master
      directories:
      - path: helmcharts/security-policy-charts/*  #creats application for each sub-directory with this singe appset manifest.
  template:
    metadata:
      name: '{{path.basename}}'
      namespace: argocd
    spec:
      project: default
      source:
        repoURL: https://github.com/mabusaa/argocd-example-apps.git
        targetRevision: master
        path: '{{path}}'
      destination:
        server: https://kubernetes.default.svc  # Should give cluster url, cannot access variable from secret object
        namespace: '{{path.basename}}'
      syncPolicy:
         syncOptions:
          - CreateNamespace=true
```


