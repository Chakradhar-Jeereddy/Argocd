## ApplicationSet
- Genrators + template
- Template defines how each app looks
- Generator defines what to loop over
- Reduces YAML duplication and creates app at scale.
It is a controller and CRD.

1) Used for automating the applications generation for many clusters.
2) More flexibility when managed Argo CD applications

## Use cases
- Single k8s manifest to deploy into multiple k8s clusters
- Singe k8s manifest to deploy multiple applications from one or more git repo

## How it works
- Appset controller only responsible of creating, updating and deleting applications in argocd namespace.
- Application ser controller does not modify k8s resources. Its application controller responsibility to deploy resources into destination cluster.

## Genrators
- Generators are reponsible for generating parameters, which are then rendered into application template.
- They are primarily based on data source that they use to generate the template parameters.

## Types of generator
| Generator Type                | Purpose                                       | Example Use Case                                         |
| ----------------------------- | --------------------------------------------- | -------------------------------------------------------- |
| **List Generator**            | Static list of applications                   | Manually define multiple environments (dev, stage, prod) |
| **Git Generator**             | Scans a Git repo for directories or files     | One app per Helm chart or per Kustomize folder           |
| **Cluster Generator**         | Creates one app per Argo CD cluster           | Deploy to multiple clusters automatically                |
| **Matrix Generator**          | Combines outputs of two or more generators    | Deploy each environment to multiple clusters             |
| **SCM Provider Generator**    | Pulls repos dynamically from GitHub/GitLab    | Auto-create apps per repo/branch                         |
| **Pull Request Generator**    | Creates apps for each PR                      | Spin up preview environments for every PR                |
| **Matrix + Merge Generators** | Combine and merge results of other generators | Advanced control for complex topologies                  |
```

apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: list-generator-example
spec:
  generators:
  - list:
      elements:
      - cluster: dev
        url: https://kubernetes.default.svc
      - cluster: prod
        url: https://prod-cluster.example.com
  template:
    metadata:
      name: '{{cluster}}-app'
    spec:
      project: default
      source:
        repoURL: https://github.com/example/app-configs.git
        path: apps/{{cluster}}
      destination:
        server: '{{url}}'
        namespace: default
```
***Creates two apps: dev-app and prod-app***
```
generators:
- git:
    repoURL: https://github.com/example/app-configs.git
    revision: main
    directories:
    - path: apps/*
```
***Argo CD creates one Application for each folder under apps/.***
```
generators:
- clusters: {}
template:
  metadata:
    name: '{{name}}-app'
  spec:
    destination:
      server: '{{server}}'
      namespace: default
    source:
      repoURL: https://github.com/example/repo.git
      path: app
```
***Automatically deploys the same app to all registered Argo CD clusters.***

```
generators:
- matrix:
    generators:
    - list:
        elements:
        - env: dev
        - env: prod
    - clusters: {}
```
***Produces all combinations of env × cluster (e.g., dev on cluster1, prod on cluster2).***
