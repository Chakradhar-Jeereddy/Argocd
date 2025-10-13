### Cluster generator
Assigment - 
1) Deploy one application into all clusters
   - Application name should include cluster name.
   - Use one applicationset manifest with cluster generator to achieve this.
2) Practice2: Deploy one application into all clusters with matching labels non-prod=true.
   - Application name should include cluster name.
   - Use one applicationset with cluster generator to achieve this.
     
Requirement - 
1) Add local and remote cluster as secert with labels in argocd namespace.
2) We can't use generator type cluster without adding cluster as secret.
3) We add labels in secret and specify matching labels in cluster generator.
4) Only when they match, the app gets created and resources gets deployed in those clusters
5) When we remove the label from sceret, the app and resources automatically gets removed from the cluster.
```
apiVersion: v1
kind: Secret
metadata:
  namespace: argocd  #namespace where you want to create appset object
  name: local-cluster       #name of the appset
  labels:
    argocd.argoproj.io/secret-type: cluster  # the cluster gets registered in argocd with this namespace
    environment: "dev"
    provider: "local"
    non-prod: "true"
type: Opaque
stringData:
  name: local-cluster                      #Giving name label to the cluster getting registered with argocd
  server: https://kubernetes.default.svc   #URL of the k8s cluster
  config: |                                 #use only secure connection
    {
      "tlsClientConfig": {
        "insecure": false
      }
    }
```
Describe secret and notice that the labels are added to it and gets displyed in web UI
```
kubectl apply -f secret.yaml
$ kubectl describe secret local-cluster
Name:         local-cluster
Namespace:    argocd
Labels:       argocd.argoproj.io/secret-type=cluster
              environment=dev
              non-prod=true
              provider=local
Annotations:  managed-by: argocd.argoproj.io

Type:  Opaque

Data
====
config:  38 bytes
name:    13 bytes
server:  30 bytes
```
Add appset to generate application
```
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: guestbook-1               # name of appset
  namespace: argocd               # namespace where app needs to be deployed
spec:
  generators:
  - clusters: {}                  #empty string means all custers, means generate app for each cluster
  template:                      
    metadata:
      name: '{{name}}-guestbook-1'   #name of the appset get value for name variable from secret
      namespace: argocd
    spec:
      project: default
      source:
        repoURL: https://github.com/mabusaa/argocd-example-apps.git
        targetRevision: master
        path: guestbook
      destination:
        server: '{{server}}'        # Get the value for variable server from secret
        namespace: guestbook-1      # Namespace in k8s where the kube manifests from git should be deployed.
      syncPolicy:
         syncOptions:
          - CreateNamespace=true         #Create the namespace where you want the resources to be deployed.
```
Create ApplicationSet
```
 ./argocd.exe appset create  appset1.yaml
ApplicationSet 'guestbook-1' created
Name:               argocd/guestbook-1
Project:            default
Server:             {{server}}
Namespace:          guestbook-1
Source:
- Repo:             https://github.com/mabusaa/argocd-example-apps.git
  Target:           master
  Path:             guestbook
SyncPolicy:         <none>
```

### Cluster generator with patching labels
```
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: guestbook-2
  namespace: argocd
spec:
  generators:
  - clusters:
     selector:
       matchLabels:
        non-prod: "true"     ##app is created only for cluster which has label "non-prod: true" in secret.

  template:
    metadata:
      name: '{{name}}-guestbook-2'
      namespace: argocd
    spec:
      project: default
      source:
        repoURL: https://github.com/mabusaa/argocd-example-apps.git
        targetRevision: master
        path: guestbook
      destination:
        server: '{{server}}'
        namespace: guestbook-2
      syncPolicy:
         syncOptions:
          - CreateNamespace=true
```
```
 ./argocd.exe appset create appset2.yaml
ApplicationSet 'guestbook-2' created
Name:               argocd/guestbook-2
Project:            default
Server:             {{server}}
Namespace:          guestbook-2
Source:
- Repo:             https://github.com/mabusaa/argocd-example-apps.git
  Target:           master
  Path:             guestbook
SyncPolicy:         <none>

./argocd.exe appset  get argocd/guestbook-2
Name:               argocd/guestbook-2
Project:            default
Server:             {{server}}
Namespace:          guestbook-2
Source:
- Repo:             https://github.com/mabusaa/argocd-example-apps.git
  Target:           master
  Path:             guestbook
SyncPolicy:         <none>

CONDITION            STATUS  MESSAGE                                                 LAST TRANSITION
ErrorOccurred        False   Successfully generated parameters for all Applications  2025-10-13 09:54:56 -0400 EDT
ParametersGenerated  True    Successfully generated parameters for all Applications  2025-10-13 09:54:56 -0400 EDT
ResourcesUpToDate    True    ApplicationSet up to date                               2025-10-13 09:54:56 -0400 EDT
```
```
 ./argocd app list
NAME                              CLUSTER                         NAMESPACE    PROJECT  STATUS     HEALTH   SYNCPOLICY  CONDITIONS  REPO
                           PATH       TARGET
argocd/local-cluster-guestbook-2  https://kubernetes.default.svc  guestbook-2  default  OutOfSync  Missing  Manual      <none>      https://github.com/mabusaa/argocd-example-apps.git  guestbook  master
```
```
./argocd app get argocd/local-cluster-guestbook-2
Name:               argocd/local-cluster-guestbook-2
Project:            default
Server:             https://kubernetes.default.svc
Namespace:          guestbook-2
URL:                https://localhost:8080/applications/local-cluster-guestbook-2
Source:
- Repo:             https://github.com/mabusaa/argocd-example-apps.git
  Target:           master
  Path:             guestbook
SyncWindow:         Sync Allowed
Sync Policy:        Manual
Sync Status:        OutOfSync from master (fa875a8)
Health Status:      Missing

GROUP  KIND        NAMESPACE    NAME          STATUS     HEALTH   HOOK  MESSAGE
       Service     guestbook-2  guestbook-ui  OutOfSync  Missing
apps   Deployment  guestbook-2  guestbook-ui  OutOfSync  Missing
```
### Now if appset matchinglabel doesn't match with the labels in secret object of the registered cluster. 
### The app and all deployed resources gets destroyed.
