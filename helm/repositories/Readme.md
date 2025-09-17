## Setting up local repository
- Create a new folder chartsrepo
  ```
  mkdir chartsrepo
  ```
- The repo should have index.yaml file
  ```
  helm repo index chartsrepo/
  ```
- Add first chart to the repository
 ```
  helm package myfirstchart -d chartsrepo
  helm repo index chartsrepo
 ```
- Host the repo on webserver
 ```
  cd chartsrepo
  python -m http.server --bind 127.0.0.1 8080
  Serving HTTP on 127.0.0.1 port 8080 (http://127.0.0.1:8080/) ...
  127.0.0.1 - - [17/Sep/2025 17:11:14] "GET / HTTP/1.1" 200 -

  browse the address http://127.0.0.1:8080/
 ```

- Downloading repo from webserver and installing the chart
```
helm repo add myrepo http://127.0.0.1:8080/
helm repo list
NAME    URL
bitnami https://charts.bitnami.com/bitnami
myrepo  http://127.0.0.1:8080/

helm install firstchat myrepo/mychart
NAME: firstapp
LAST DEPLOYED: Wed Sep 17 17:47:49 2025
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services firstapp-mychart)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
```


 
  
