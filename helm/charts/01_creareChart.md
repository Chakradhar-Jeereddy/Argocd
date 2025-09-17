## Create a hlem chart
```
helm create mychart

ls
Chart.yaml  charts/  templates/  values.yaml
```

1. chart.yaml > Metadata
2. values.yaml > Default values can be modified
3. templates
   * deployment.yaml
   *  hpa.yaml
   *  ingress.yaml
   *  service.yaml
   *  serviceaccount.yaml
   *  Notes.txt
   *  _helpers.tpl
 4. charts(dir) > empty folder

## By default the chart will have values to deploy nginx
## Install the first app
```
helm install myapp mychart
NAME: myapp
LAST DEPLOYED: Wed Sep 17 03:04:08 2025
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=mychart,app.kubernetes.io/instance=myapp" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT

```
## Run the below commands to enable port farwording to access locally.

```
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=mychart,app.kubernetes.io/instance=myapp" 
-o jsonpath="{.items[0].metadata.name}")
export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")

kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
Handling connection for 8080
Handling connection for 8080

Access the URL:  http://127.0.0.1:8080
```
