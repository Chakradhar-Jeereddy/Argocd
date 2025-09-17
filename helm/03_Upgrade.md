## Upgrade
  - It will push only new changes to the deployment.
  - Upgrade --install (If the deployment doesn't exist it will install, if exist, it will upgrade)
```
helm upgrade mydb bitnami/mysql --values values.yaml

helm list (It will display the deployments done through helm, show application version and chart version)
help repo list (It will display the repo name and the URL)

helm upgrade mydb bitnami/mysql --reuse-values  (helm maintains the values supplied during installation and can reuse that)
```

## Example of upgrade --install
```
helm upgrade --install mydb bitnami/apache --values values.yaml
Release "mydb" does not exist. Installing it now.

helm upgrade --install mydb bitnami/apache --values values.yaml
Release "mydb" has been upgraded. Happy Helming!
```


