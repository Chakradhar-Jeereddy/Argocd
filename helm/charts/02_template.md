## Template is heart of any chart
- It is used to generate kubernetes manifest files.
- Default templates are
    * Deployment.yaml
    * hpa.yaml
    * Ingress.yaml
    * Servcie.yaml
    * serviceaccount.yaml
- We can add template and remove templates based on our requirements.

## _helpers.tpl
- This file wont generate kube manifests.
- Functions are declared in this files which will used by templates to genetate manifest files

## Example
```
apiVersion: v1
kind: Service
metadata:
  name: {{ include "mychart.fullname" . }}
  labels:
    {{- include "mychart.labels" . | nindent 4 }}
spec:
  type: {{ .Values.service.type }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: http
      protocol: TCP
      name: http
  selector:
    {{- include "mychart.selectorLabels" . | nindent 4 }}
```

## Template actions
- Syntax is {{ action }} or {{- action }} to remove any white space.
- Action can print or call a function.
## Example
```
apiVersion: apps/v1
kind: Deployment
  {{ "I'm great" -}}, {{ "I'm the best" }}
metadata:
  name: {{ include "mychart.fullname" . }}
  labels:

Output:

apiVersion: apps/v1
kind: Deployment
  I'm great, I'm the best

```

## retriving data from chat.yaml, template, values.yaml and release

dot is the root object has all data.
.values is a subject under root.
```
vi mychart/template/depoyment.yaml
{{.Values.replicaCount }}
{{.Values.autoscaling.enabled }}
{{.Values.serviceAccount.create}}
{{.Chart.Version}}
{{.Chart.Name}
{{.Release.Service}}
{{.Release.Namespace}}
{{.Release.IsInstall}}
{{.Release.IsUpgrade}}
{{.Template.Name}}
{{.Template.BasePath}}

helm lint mychart
helm template mychart

apiVersion: apps/v1
kind: Deployment
  1
  false
  true
  0.1.0
  mychart
  Helm
  default
  true
  false

````

## Pipeline
- It transforms the input value with the help of functions
- {{.Values.my.custom.data | default "chakra" | upper | quote}}
```
vi deployment.yaml
 {{.Values.my.custom.data | default "chakra" | upper | quote}}

vi values.yaml
 my:
  custom:
   data:

helm template mychart
kind: Deployment
  1
  false
  true
  0.1.0
  mychart
  Helm
  default
  true
  false
  mychart/templates/deployment.yaml
  mychart/templates
  "CHAKRA"   <<<<<<<<<<
```

## Functions used for transformation
- nindent 4 > Means new line with indentation 4 (4 tabs)
- toYaml convert the output into yaml

## Conditional logic if, if else, if not
{{- if not .Values.autoscaling.enabled }}
  replicas: {{ .Values.replicaCount }}
  {{- end }}

{{- if .Values.autoscaling.targetMemoryUtilizationPercentage }}
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: {{ .Values.autoscaling.targetMemoryUtilizationPercentage }}
    {{- end }}

## With clause is used for list objects, it will print the list like annotation
-- Example
{{-with .Values.podAnnotations}}
annotations:
 {{-toYaml .| nindent 8 }}
 {{-end}}

## Variables in template
- {{ $myFalg := .Values.my.flag }}

## Loops
- Same has with but uses the range keyword
```
Countries of deployment
{{- range .Values.my.values}
 - {{. | upper | quote}}
{{- end}}

- Loop dict types
{{- range $key,$value.Values.image}
 - {{$key}}: {{$value}}
{{- end}}
```

