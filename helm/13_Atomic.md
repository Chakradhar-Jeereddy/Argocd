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
