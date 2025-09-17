## Helm maintains version history in the form of secret objects in kubernetes.
 - Every installation and upgrade creates a secret object.
```
$ kubectl get secrets
NAME                         TYPE                 DATA   AGE
mydb-mysql                   Opaque               2      30m
sh.helm.release.v1.mydb.v1   helm.sh/release.v1   1      30m
sh.helm.release.v1.mydb.v2   helm.sh/release.v1   1      13m
sh.helm.release.v1.mydb.v3   helm.sh/release.v1   1      2m20s
```

## Uninstall will remove the secrets also, to keep the secerts use the keep option
```
kubectl get secrets
NAME                         TYPE                 DATA   AGE
mydb-mysql                   Opaque               2      27s
sh.helm.release.v1.mydb.v1   helm.sh/release.v1   1      28s
sh.helm.release.v1.mydb.v2   helm.sh/release.v1   1      7s

helm list
helm uninstall mydb --keep-history

kubectl get secrets
NAME                         TYPE                 DATA   AGE
sh.helm.release.v1.mydb.v1   helm.sh/release.v1   1      54s
sh.helm.release.v1.mydb.v2   helm.sh/release.v1   1      33s
helm list

kubectl delete secret --all
secret "sh.helm.release.v1.mydb.v1" deleted
secret "sh.helm.release.v1.mydb.v2" deleted

```
