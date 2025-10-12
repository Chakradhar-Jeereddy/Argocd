## ApplicationSet
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
1 List generator
2 Cluster generator
3 Git generator
4 Matrix generator
5 Merge generator
6 SCM provider generator
7 Pull request generator
8 Cluster decision resource generator







- 

