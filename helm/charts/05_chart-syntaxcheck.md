## Use lint command to check the syntax of the chart.
 - It checks all files and retruns zero when synax is valid.
 - For errors it returns non-zero code.

## Example
```
 helm lint mychart
==> Linting mychart
[INFO] Chart.yaml: icon is recommended

1 chart(s) linted, 0 chart(s) failed

=> Generate a synatx eror in deployment.yaml and re-run the lint

helm lint mychart
==> Linting mychart
[INFO] Chart.yaml: icon is recommended
[ERROR] templates/deployment.yaml: unable to parse YAML: error converting YAML to JSON: yaml: line 5: mapping values are not allowed in this context

Error: 1 chart(s) linted, 1 chart(s) failed

```


