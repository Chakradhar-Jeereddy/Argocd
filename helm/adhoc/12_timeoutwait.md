## Helm waits untill the resrouces are up and running
## Timeout to exit after the time is elapsed.
```
helm upgrade --install mydb bitnami/apache --values values.yaml --wait --timeout 5m10s
Release "mydb" does not exist. Installing it now.
```
