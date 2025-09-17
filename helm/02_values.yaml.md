```
 cat values.yaml
auth:
  rootPassword: "test1234"

=> Supply value files

helm install mydb bitnami/mysql --set auth.rootPassword = test1234 (not recommended)

helm install mydb bitnami/mysql --values values.yaml

=> Extract the password
kubectl get secret --namespace default mydb-mysql -o jsonpath="{.data.mysql-root-password}" | base64 -d
kubectl get secret mydb-mysql -o custom-columns=DATA:.data.root-password|tail -1|base64 -d
```
