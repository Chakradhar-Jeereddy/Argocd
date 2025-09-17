## Upgrade
  - It will push only new changes to the deployment.
```
helm upgrade mydb bitnami/mysql --values values.yaml

helm list (It will display the deployments done through helm, show application version and chart version)
help repo list (It will display the repo name and the URL)

helm upgrade mydb bitnami/mysql --reuse-values  (helm maintains the values supplied during installation and can reuse that)
```
