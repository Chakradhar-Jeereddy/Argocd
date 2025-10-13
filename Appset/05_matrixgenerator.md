## Matrix generator
- Deploy multiple applications into all clusters that has label non-prod=true
- Application name and namespace should include base path name that's in git and cluster-name where
- it deployed into.
- Use one applicationset manifest with matrix generator to achieve this.
- Deploy all helm charts under path (helmcharts/security-policy-charts).
- https://github.com/mabusaa/argocd-course-apps-definitions/blob/main/application%20set/matrix-generator.yaml

```
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: security-apps-matrix
  namespace: argocd
spec:
  generators:
  - matrix:
      generators:
      - git:
          repoURL: https://github.com/mabusaa/argocd-example-apps.git
          revision: master
          directories:
          - path: helmcharts/security-policy-charts/*
      - clusters:
          selector:
            matchLabels:
              non-prod: "true"
  template:
    metadata:
      name: '{{ nameNormalized }}-{{path.basename}}'
      namespace: argocd
    spec:
      project: default
      source:
        repoURL: https://github.com/mabusaa/argocd-example-apps.git
        targetRevision: master
        path: '{{path}}'
      destination:
        server: '{{server}}'                         #Cluster generator gets it from secret object.
        namespace: '{{ nameNormalized }}-{{path.basename}}'
      syncPolicy:
         syncOptions:
          - CreateNamespace=true
```
