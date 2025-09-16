## Interact ArgoCD with
   * CLI
   * Web UI
   * Rest/gRPC API

- CLI is useful when you need to interact with ArgoCD in CI pipelines.

## Managing using CLI
  * Manage applications.
  * Manage Repos.
  * Manage clusters.
  * Admin tasks.
  * Manage projects
  * And more.

## Instructions to install
 - https://argo-cd.readthedocs.io/en/stable/cli_installation/

## Login to arocd using client-cli and run interactive commandsPS C:\Users\chakr> .\argocd login localhost:8080
```
WARNING: server certificate had error: tls: failed to verify certificate: x509: certificate signed by unknown authority. Proceed insecurely (y/n)? y
Username: admin
Password:
'admin:login' logged in successfully
Context 'localhost:8080' updated

.\argocd app list
NAME  CLUSTER  NAMESPACE  PROJECT  STATUS  HEALTH  SYNCPOLICY  CONDITIONS  REPO  PATH  TARGET
.\argocd appset list
NAME  PROJECT  SYNCPOLICY  CONDITIONS  REPO  PATH  TARGET
```



