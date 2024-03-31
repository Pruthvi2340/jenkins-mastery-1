# Jenkins installation in goolge kubernetes engine

```
helm repo add jenkins https://charts.jenkins.io
helm repo update
helm upgrade --install -f jenkins/values.yaml myjenkins jenkins/jenkins
```
**values.yaml**
```
controller:
  installPlugins:
    - kubernetes:latest
    - workflow-job:latest
    - workflow-aggregator:latest
    - credentials-binding:latest
    - git:latest
    - google-oauth-plugin:latest
    - google-source-plugin:latest
    - google-kubernetes-engine:latest
    - google-storage-plugin:latest
  resources:
    requests:
      cpu: "50m"
      memory: "1024Mi"
    limits:
      cpu: "1"
      memory: "3500Mi"
  javaOpts: "-Xms3500m -Xmx3500m"
  serviceType: ClusterIP
agent:
  resources:
    requests:
      cpu: "500m"
      memory: "256Mi"
    limits:
      cpu: "1"
      memory: "512Mi"
persistence:
  size: 100Gi
serviceAccount:
  name: cd-jenkins
```

**Port Forward**
```
echo http://127.0.0.1:8080
kubectl --namespace default port-forward svc/myjenkins 8080:8080 >> /dev/null &
```

**Password of jenkins**
```
kubectl exec --namespace default -it svc/myjenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo
```
**list all resource created by helm**
```
helm list
helm get all myjenkin
helm show values jenkins/jenkins
```
**Get all values passed to the helm repo**
```
helm get values myjenkins -a -n default > values.yaml
```
