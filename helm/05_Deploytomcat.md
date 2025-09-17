```
cat values.yaml
tomcatPassword: test
service:
  type: NodePort
  nodePorts:
    http: 30007

helm install tomcat oci://registry-1.docker.io/bitnamicharts/tomcat --values values.yaml

helm list
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
tomcat  default         1               2025-09-16 22:56:02.2863936 -0400 EDT   deployed        tomcat-12.0.8   11.0.10

kubectl get secrets
NAME                           TYPE                 DATA   AGE
sh.helm.release.v1.tomcat.v1   helm.sh/release.v1   1      7m6s
tomcat                         Opaque               2      7m5s

helm repo list
NAME    URL
bitnami https://charts.bitnami.com/bitnami

helm uninstall tomcat
release "tomcat" uninstalled
```



