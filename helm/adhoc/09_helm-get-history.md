## helm get is sub command of helm list
```
helm get notes tomcat  (gets release notes)

NOTES:
CHART NAME: tomcat
CHART VERSION: 12.0.8
APP VERSION: 11.0.10

helm get values tomcat

USER-SUPPLIED VALUES:
service:
  nodePorts:
    http: 30007
  type: NodePort
tomcatPassword: test

 helm get values tomcat --all(gets all default values)

helm get values tomcat --revision=0
USER-SUPPLIED VALUES:
service:
  nodePorts:
    http: 30007
  type: NodePort
tomcatPassword: test
```

# helm history (it also keeps failure information.

```
helm history tomcat
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Wed Sep 17 00:08:37 2025        superseded      tomcat-12.0.8   11.0.10         Install complete
2               Wed Sep 17 00:08:51 2025        deployed        tomcat-12.0.8   11.0.10         Upgrade complete
```

## Example

```
helm install mydb bitnami/apache --set image.pullPolicy=test
Error: INSTALLATION FAILED: 1 error occurred:
        * Deployment.apps "mydb-apache" is invalid: [spec.template.spec.containers[0].imagePullPolicy: Unsupported value: "test": supported values: "Always", "IfNotPresent", "Never", spec.template.spec.initContainers[0].imagePullPolicy: Unsupported value: "test": supported values: "Always", "IfNotPresent", "Never"]

$ helm history
Error: "helm history" requires 1 argument

Usage:  helm history RELEASE_NAME [flags]

$ helm history mydb
REVISION        UPDATED                         STATUS  CHART           APP VERSION     DESCRIPTION


1               Wed Sep 17 00:25:14 2025        failed  apache-11.4.29  2.4.65          Release "mydb" failed: 1 error occurred:


                                                                                                * Deployment.apps "mydb-apache" is invalid: [spec.template.spec.containers[0].imagePullPolicy: Unsupported value: "test": supported values: "Always", "IfNotPresent", "Never", spec.template.spec.initContainers[0].imagePullPolicy: Unsupported value: "test": supported...
```

