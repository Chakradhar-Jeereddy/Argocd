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


