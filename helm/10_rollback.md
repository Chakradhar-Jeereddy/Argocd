## Rollback creates another secret.
```
helm install mydb bitnami/apache --values values.yaml
helm upgrade mydb bitnami/apache --values values.yaml

helm rollback mydb 1
Rollback was a success! Happy Helming!

kubectl get secret
NAME                         TYPE                 DATA   AGE
sh.helm.release.v1.mydb.v1   helm.sh/release.v1   1      97s
sh.helm.release.v1.mydb.v2   helm.sh/release.v1   1      83s
sh.helm.release.v1.mydb.v3   helm.sh/release.v1   1      6s


helm history mydb
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Wed Sep 17 00:35:12 2025        superseded      apache-11.4.29  2.4.65          Install complete
2               Wed Sep 17 00:35:26 2025        superseded      apache-11.4.29  2.4.65          Upgrade complete
3               Wed Sep 17 00:36:43 2025        deployed        apache-11.4.29  2.4.65          Rollback to 1
```

