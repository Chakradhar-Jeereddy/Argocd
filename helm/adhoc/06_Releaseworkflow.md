## What happens when we deploy application using helm?
1. Loads the chart and its dependencies
2. Parse the value files
3. Generate the yaml
4. Parse the YAML to kube objects and validate
5. Generate YAML and send to kube.
