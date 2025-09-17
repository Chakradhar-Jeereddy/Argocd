## If you don't want the CI pipeline to rollback incase of failure use the atomic clause.
- It by defaults enables --wait of 5 minutes, we can increase it using --timeout=1m1s
```
helm upgrade mydb bitnami/apache --set image.pullPolicy=test --atomic
Error: UPGRADE FAILED: release mydb failed, and has been rolled back due to atomic being set: cannot patch
 "mydb-apache" with kind Deployment: Deployment.apps "mydb-apache" is invalid: [spec.template.spec.containers[0].imagePullPolicy:
Unsupported value: "test": supported values: "Always", "IfNotPresent", "Never", spec.template.spec.initContainers[0].imagePullPolicy:
Unsupported value: "test": supported values: "Always", "IfNotPresent", "Never"]

OR use timeout to reduce the wait time.

helm upgrade mydb bitnami/apache --set image.pullPolicy=test --atomic --timeout=1s
Error: UPGRADE FAILED: release mydb failed, and has been rolled back due to atomic being set:
cannot patch "mydb-apache" with kind Deployment: Deployment.apps "mydb-apache" is invalid:
[spec.template.spec.containers[0].imagePullPolicy: Unsupported value: "test": supported values: "Always", "IfNotPresent",
"Never", spec.template.spec.initContainers[0].imagePullPolicy: Unsupported value: "test": supported values: "Always", "IfNotPresent", "Never"]

```
## Check history
```
helm history mydb
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION


1               Wed Sep 17 01:15:06 2025        superseded      apache-11.4.29  2.4.65          Install complete


2               Wed Sep 17 01:15:45 2025        failed          apache-11.4.29  2.4.65          Upgrade "mydb" failed: cannot patch "mydb-apache"
with kind Deployment: Deployment.apps "mydb-apache" is invalid: [spec.template.spec.containers[0].imagePullPolicy: Unsupported value: "test":
supported values: "Always", "IfNotPresent", "Never", spec.template.spec.initContainers[0].imagePullPolicy: Unsupported value: "test": supported values: "Always", "IfNotPresent", "Never"]
3               Wed Sep 17 01:15:47 2025        superseded      apache-11.4.29  2.4.65          Rollback to 1


4               Wed Sep 17 01:18:43 2025        failed          apache-11.4.29  2.4.65          Upgrade "mydb" failed: cannot patch "mydb-apache"
with kind Deployment: Deployment.apps "mydb-apache" is invalid: [spec.template.spec.containers[0].imagePullPolicy: Unsupported value: "test": supported values: "Always", "IfNotPresent", "Never", spec.template.spec.initContainers[0].imagePullPolicy: Unsupported value: "test": supported values: "Always", "IfNotPresent", "Never"]
5               Wed Sep 17 01:18:44 2025        deployed        apache-11.4.29  2.4.65          Rollback to 3
```
