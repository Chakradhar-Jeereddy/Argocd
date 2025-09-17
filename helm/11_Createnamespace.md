## We can create namespace from helm client using --create-namespace and --namespace.
- Delete namespace is not allowed from helm client
```
helm install mydb bitnami/apache --values values.yaml --namespace chakra --create-namespace

helm list -n chakra
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
mydb    chakra          1               2025-09-17 00:44:53.905922 -0400 EDT    deployed        apache-11.4.29  2.4.65

```
