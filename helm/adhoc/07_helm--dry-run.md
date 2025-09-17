## Dry run does everything except deployment
## It is very powerfull to debug.
- It parse the value files
- Generates the yaml   (It communicates with kube API and validate the generated manifest files). Need connection to kube cluster).
- Validates the yaml
- Dumps the gernetated manifest files

```
helm install tomcat oci://registry-1.docker.io/bitnamicharts/tomcat --values values.yaml --dry-run
```
## Information section
```
Pulled: registry-1.docker.io/bitnamicharts/tomcat:12.0.8
Digest: sha256:8f84c84063db9389169dd697b282501d9bf3d22557bcf925814d61a4066a731c
NAME: tomcat
LAST DEPLOYED: Tue Sep 16 23:25:37 2025
NAMESPACE: default
STATUS: pending-install
REVISION: 1
TEST SUITE: None
HOOKS:
MANIFEST:
---
## Manifest files
```
# Source: tomcat/templates/networkpolicy.yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: tomcat
  namespace: "default"
  labels:
    app.kubernetes.io/instance: tomcat
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: tomcat
    app.kubernetes.io/version: 11.0.10
    helm.sh/chart: tomcat-12.0.8
spec:
  podSelector:
    matchLabels:
      app.kubernetes.io/instance: tomcat
      app.kubernetes.io/name: tomcat
  policyTypes:
    - Ingress
    - Egress
  egress:
    - {}
  ingress:
    - ports:
        - port: 8080
          protocol: TCP
---
# Source: tomcat/templates/pdb.yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: tomcat
  namespace: default
  labels:
    app.kubernetes.io/instance: tomcat
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: tomcat
    app.kubernetes.io/version: 11.0.10
    helm.sh/chart: tomcat-12.0.8
spec:
  maxUnavailable: 1
  selector:
    matchLabels:
      app.kubernetes.io/instance: tomcat
      app.kubernetes.io/name: tomcat
---
# Source: tomcat/templates/serviceaccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tomcat
  namespace: "default"
  labels:
    app.kubernetes.io/instance: tomcat
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: tomcat
    app.kubernetes.io/version: 11.0.10
    helm.sh/chart: tomcat-12.0.8
automountServiceAccountToken: false
---
# Source: tomcat/templates/secrets.yaml
apiVersion: v1
kind: Secret
metadata:
  name: tomcat
  namespace: default
  labels:
    app.kubernetes.io/instance: tomcat
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: tomcat
    app.kubernetes.io/version: 11.0.10
    helm.sh/chart: tomcat-12.0.8
type: Opaque
data:
  tomcat-username: "dXNlcg=="
  tomcat-password: "dGVzdA=="
---
# Source: tomcat/templates/pvc.yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: tomcat
  namespace: default
  labels:
    app.kubernetes.io/instance: tomcat
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: tomcat
    app.kubernetes.io/version: 11.0.10
    helm.sh/chart: tomcat-12.0.8
spec:
  accessModes:
    - "ReadWriteOnce"
  resources:
    requests:
      storage: "8Gi"
---
# Source: tomcat/templates/svc.yaml
apiVersion: v1
kind: Service
metadata:
  name: tomcat
  namespace: default
  labels:
    app.kubernetes.io/instance: tomcat
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: tomcat
    app.kubernetes.io/version: 11.0.10
    helm.sh/chart: tomcat-12.0.8
spec:
  type: NodePort
  externalTrafficPolicy: "Cluster"
  sessionAffinity: None
  ports:
    - name: http
      port: 80
      targetPort: http
      nodePort: 30007
  selector:
    app.kubernetes.io/instance: tomcat
    app.kubernetes.io/name: tomcat
---
# Source: tomcat/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat
  namespace: default
  labels:
    app.kubernetes.io/instance: tomcat
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: tomcat
    app.kubernetes.io/version: 11.0.10
    helm.sh/chart: tomcat-12.0.8
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/instance: tomcat
      app.kubernetes.io/name: tomcat
  strategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app.kubernetes.io/instance: tomcat
        app.kubernetes.io/managed-by: Helm
        app.kubernetes.io/name: tomcat
        app.kubernetes.io/version: 11.0.10
        helm.sh/chart: tomcat-12.0.8
    spec:

      automountServiceAccountToken: false
      affinity:
        podAffinity:

        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - podAffinityTerm:
                labelSelector:
                  matchLabels:
                    app.kubernetes.io/instance: tomcat
                    app.kubernetes.io/name: tomcat
                topologyKey: kubernetes.io/hostname
              weight: 1
        nodeAffinity:

      serviceAccountName: tomcat
      securityContext:
        fsGroup: 1001
        fsGroupChangePolicy: Always
        supplementalGroups: []
        sysctls: []
      initContainers:
      containers:
        - name: tomcat
          image: docker.io/bitnami/tomcat:11.0.10-debian-12-r4
          imagePullPolicy: "IfNotPresent"
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop:
              - ALL
            privileged: false
            readOnlyRootFilesystem: true
            runAsGroup: 1001
            runAsNonRoot: true
            runAsUser: 1001
            seLinuxOptions: {}
            seccompProfile:
              type: RuntimeDefault
          env:
            - name: BITNAMI_DEBUG
              value: "false"
            - name: TOMCAT_USERNAME_FILE
              value: /opt/bitnami/tomcat/secrets/tomcat-username
            - name: TOMCAT_PASSWORD_FILE
              value: /opt/bitnami/tomcat/secrets/tomcat-password
            - name: TOMCAT_ALLOW_REMOTE_MANAGEMENT
              value: "0"
            - name: TOMCAT_HTTP_PORT_NUMBER
              value: "8080"
          ports:
            - name: http
              containerPort: 8080
          livenessProbe:
            tcpSocket:
              port: http
            failureThreshold: 6
            initialDelaySeconds: 120
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 5
          readinessProbe:
            httpGet:
              path: /
              port: http
            failureThreshold: 3
            initialDelaySeconds: 30
            periodSeconds: 5
            successThreshold: 1
            timeoutSeconds: 3
          resources:
            limits:
              cpu: 375m
              ephemeral-storage: 2Gi
              memory: 384Mi
            requests:
              cpu: 250m
              ephemeral-storage: 50Mi
              memory: 256Mi
          volumeMounts:
            - name: data
              mountPath: /bitnami/tomcat
            - name: empty-dir
              mountPath: /opt/bitnami/tomcat/temp
              subPath: app-tmp-dir
            - name: empty-dir
              mountPath: /opt/bitnami/tomcat/conf
              subPath: app-conf-dir
            - name: empty-dir
              mountPath: /opt/bitnami/tomcat/logs
              subPath: app-logs-dir
            - name: empty-dir
              mountPath: /opt/bitnami/tomcat/work
              subPath: app-work-dir
            - name: empty-dir
              mountPath: /tmp
              subPath: tmp-dir
            - name: tomcat-secrets
              mountPath: /opt/bitnami/tomcat/secrets
      volumes:
        - name: empty-dir
          emptyDir: {}
        - name: tomcat-secrets
          projected:
            sources:
              - secret:
                  name: tomcat
        - name: data
          persistentVolumeClaim:
            claimName: tomcat
```

RELEASE NOTES:
CHART NAME: tomcat
CHART VERSION: 12.0.8
APP VERSION: 11.0.10

⚠ WARNING: Since August 28th, 2025, only a limited subset of images/charts are available for free.
    Subscribe to Bitnami Secure Images to receive continued support and security updates.
    More info at https://bitnami.com and https://github.com/bitnami/containers/issues/83267

** Please be patient while the chart is being deployed **

1. Get the Tomcat URL by running:

  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services tomcat)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo "Tomcat URL:            http://$NODE_IP:$NODE_PORT"
  echo "Tomcat Management URL: http://$NODE_IP:$NODE_PORT/manager"

2. Login with the following credentials

  echo Username: user
  echo Password: $(kubectl get secret --namespace default tomcat -o jsonpath="{.data.tomcat-password}" | base64 -d)

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
