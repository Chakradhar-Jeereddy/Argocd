## Prerequisite
- Need a running K8S cluster (minkube, docker desktop, kind, rancher desktop or full cluster).

## Installation types
1. Non-HA setup suitable for evaluation or dev/testing environments, each component with one pod is created.
2. HA setup is recommnded for production, needs 3 worker nodes. 3 Pods for each component.
3. Light installation "Core" - Suitable if ArgoCD is used by administrators only. without UI and API, by default its installed as NON-HA.

## Privilege options
- ArgoCD provides two options for in-cluster privileges
1. Cluster-admin privileges: ArgoCD will have cluster admin access to deploy into the cluster that runs in.
2. Namespaced level privileges: USe this manifest set if you do not need Argo CD to deploy applications in the same cluster that argo CD runs in.

## Manifests installation options
- Yaml manifests

## Installation repos

1. cluster-admin privileges(Non-HA): https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
2. namespace level privileges(NOn_HA): https://github.com/argoproj/argo-cd/raw/stable/manifests/namespace-install.yaml
3. cluster-admin privileges(HA): https://github.com/argoproj/argo-cd/raw/stable/manifests/ha/install.yaml
4. namespace level privileges(HA): https://github.com/argoproj/argo-cd/raw/stable/manifests/ha/namespace-install.yaml
5. Light installation "Core": https://github.com/argoproj/argo-cd/raw/stable/manifests/core-install.yaml
6. Helm chart: https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd

## Installation steps
1. You need to create a namespace for argocd.
=> kubectl create ns argocd
2. Deploy the install.yaml
=> kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
C:\Users\chakr>kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
customresourcedefinition.apiextensions.k8s.io/applications.argoproj.io unchanged
customresourcedefinition.apiextensions.k8s.io/applicationsets.argoproj.io unchanged
customresourcedefinition.apiextensions.k8s.io/appprojects.argoproj.io unchanged
serviceaccount/argocd-application-controller created
serviceaccount/argocd-applicationset-controller created
serviceaccount/argocd-dex-server created
serviceaccount/argocd-notifications-controller created
serviceaccount/argocd-redis created
serviceaccount/argocd-repo-server created
serviceaccount/argocd-server created
role.rbac.authorization.k8s.io/argocd-application-controller created
role.rbac.authorization.k8s.io/argocd-applicationset-controller created
role.rbac.authorization.k8s.io/argocd-dex-server created
role.rbac.authorization.k8s.io/argocd-notifications-controller created
role.rbac.authorization.k8s.io/argocd-redis created
role.rbac.authorization.k8s.io/argocd-server created
clusterrole.rbac.authorization.k8s.io/argocd-application-controller unchanged
clusterrole.rbac.authorization.k8s.io/argocd-applicationset-controller unchanged
clusterrole.rbac.authorization.k8s.io/argocd-server unchanged
rolebinding.rbac.authorization.k8s.io/argocd-application-controller created
rolebinding.rbac.authorization.k8s.io/argocd-applicationset-controller created
rolebinding.rbac.authorization.k8s.io/argocd-dex-server created
rolebinding.rbac.authorization.k8s.io/argocd-notifications-controller created
rolebinding.rbac.authorization.k8s.io/argocd-redis created
rolebinding.rbac.authorization.k8s.io/argocd-server created
clusterrolebinding.rbac.authorization.k8s.io/argocd-application-controller unchanged
clusterrolebinding.rbac.authorization.k8s.io/argocd-applicationset-controller unchanged
clusterrolebinding.rbac.authorization.k8s.io/argocd-server unchanged
configmap/argocd-cm created
configmap/argocd-cmd-params-cm created
configmap/argocd-gpg-keys-cm created
configmap/argocd-notifications-cm created
configmap/argocd-rbac-cm created
configmap/argocd-ssh-known-hosts-cm created
configmap/argocd-tls-certs-cm created
secret/argocd-notifications-secret created
secret/argocd-secret created
service/argocd-applicationset-controller created
service/argocd-dex-server created
service/argocd-metrics created
service/argocd-notifications-controller-metrics created
service/argocd-redis created
service/argocd-repo-server created
service/argocd-server created
service/argocd-server-metrics created
deployment.apps/argocd-applicationset-controller created
deployment.apps/argocd-dex-server created
deployment.apps/argocd-notifications-controller created
deployment.apps/argocd-redis created
deployment.apps/argocd-repo-server created
deployment.apps/argocd-server created
statefulset.apps/argocd-application-controller created
networkpolicy.networking.k8s.io/argocd-application-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-applicationset-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-dex-server-network-policy created
networkpolicy.networking.k8s.io/argocd-notifications-controller-network-policy created
networkpolicy.networking.k8s.io/argocd-redis-network-policy created
networkpolicy.networking.k8s.io/argocd-repo-server-network-policy created
networkpolicy.networking.k8s.io/argocd-server-network-policy created
```
3. Switch to arogcd namespace and check the components
```
  kubectl config set-context --current --namespace=argocd
  Context "do-nyc1-chakra1-k8s" modified.
```

4. Verify all the deployed components of argocd are up and running.
```
C:\Users\chakr>kubectl get all
NAME                                                    READY   STATUS    RESTARTS        AGE
pod/argocd-application-controller-0                     1/1     Running   0               4m47s
pod/argocd-applicationset-controller-54f96997f8-8r6r7   1/1     Running   0               4m47s
pod/argocd-dex-server-798cbff4c7-zgpnc                  1/1     Running   2 (4m38s ago)   4m47s
pod/argocd-notifications-controller-644f66f7df-6ns8g    1/1     Running   0               4m47s
pod/argocd-redis-6684c6947f-2zmcs                       1/1     Running   0               4m47s
pod/argocd-repo-server-6fccc5759b-j95gk                 1/1     Running   0               4m47s
pod/argocd-server-64d5fcbd58-7tb7w                      1/1     Running   0               4m47s

NAME                                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
service/argocd-applicationset-controller          ClusterIP   10.245.158.5     <none>        7000/TCP,8080/TCP            4m48s
service/argocd-dex-server                         ClusterIP   10.245.149.1     <none>        5556/TCP,5557/TCP,5558/TCP   4m48s
service/argocd-metrics                            ClusterIP   10.245.125.119   <none>        8082/TCP                     4m48s
service/argocd-notifications-controller-metrics   ClusterIP   10.245.173.171   <none>        9001/TCP                     4m48s
service/argocd-redis                              ClusterIP   10.245.91.67     <none>        6379/TCP                     4m48s
service/argocd-repo-server                        ClusterIP   10.245.114.31    <none>        8081/TCP,8084/TCP            4m48s
service/argocd-server                             ClusterIP   10.245.29.0      <none>        80/TCP,443/TCP               4m47s
service/argocd-server-metrics                     ClusterIP   10.245.197.167   <none>        8083/TCP                     4m47s

NAME                                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/argocd-applicationset-controller   1/1     1            1           4m47s => Gets generated manifest files from repo server and current state of cluster from k8s
deployment.apps/argocd-dex-server                  1/1     1            1           4m47s=> Integrate external identity service
deployment.apps/argocd-notifications-controller    1/1     1            1           4m47s=> Send notifications
deployment.apps/argocd-redis                       1/1     1            1           4m47s=> for caching
deployment.apps/argocd-repo-server                 1/1     1            1           4m47s=> clone repo and generates manifest files
deployment.apps/argocd-server                      1/1     1            1           4m47s => web server to perform delete/update/create via WEB UI, API and CLI

NAME                                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/argocd-applicationset-controller-54f96997f8   1         1         1       4m47s
replicaset.apps/argocd-dex-server-798cbff4c7                  1         1         1       4m47s
replicaset.apps/argocd-notifications-controller-644f66f7df    1         1         1       4m47s
replicaset.apps/argocd-redis-6684c6947f                       1         1         1       4m47s
replicaset.apps/argocd-repo-server-6fccc5759b                 1         1         1       4m47s
replicaset.apps/argocd-server-64d5fcbd58                      1         1         1       4m47s

NAME                                             READY   AGE
statefulset.apps/argocd-application-controller   1/1     4m47s
```
5. Get Initial admin password
```
kubectl get secrets
NAME                          TYPE     DATA   AGE
argocd-initial-admin-secret   Opaque   1      10m
argocd-notifications-secret   Opaque   0      11m
argocd-redis                  Opaque   1      10m
argocd-secret                 Opaque   5      11m

kubectl get secrets argocd-initial-admin-secret -o yaml
apiVersion: v1
data:
  password: RHo2Q0V2REFlWVloZ0JQaw==
kind: Secret
metadata:
  creationTimestamp: "2025-09-16T02:41:07Z"
  name: argocd-initial-admin-secret
  namespace: argocd
  resourceVersion: "27186589"
  uid: 240ae557-4fa8-420e-b745-19269ae1c959
type: Opaque

echo "RHo2Q0V2REFlWVloZ0JQaw=="|base64 -d
```

5. Access ArgoCD UI by exposing argocd server, by defaul it is not exposed with external endpoint.
Exposed by using:
 a. Service: LoadBalancer
   Change the argocd-server service type to LoadBalancer
 b. Ingress: Use your preferred ingress controller
   Create an ingress resource that point into argocd-server service.
 c. Port-forward: Simply use this to access locally on you machine
```
    kubectl port-farword svc/argocd-server -n argocd 8080
```

6. Open the browser https://localhost:8080/
    Username: admin




