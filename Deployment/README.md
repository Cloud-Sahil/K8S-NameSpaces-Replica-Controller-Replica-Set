# Kubernetes Deployment
---
A **Deployment** ensures a specified number of pod replicas are running at any given time. Deployments are best suited for stateless applications.

**Features of Deployments**
 - Stateless nature, meaning all pods are interchangeable.
 - Easy scaling and updates with zero downtime.
 - Fast rollback capability.
 - Pods are recreated with new identities upon termination.


**Use Cases**
- Web servers.
- APIs.
 - Microservices with no data dependency.

---
## Deployment (Rolling Update)
```sh
nano deployment.yaml
```
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 6
  selector:
    matchLabels:
      app: my-app
  minReadySeconds: 5
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-pod-game
        image: mukunddeo9325/super-mario
        ports:
        - containerPort: 80
```

### Apply 
```sh
kubectl apply -f deployment.yaml 
```
### Check pods
```sh
kubectl get pods
```
### Check Deployment
```sh
kubectl get deployment
```
### Expose Deployment
```sh
kubectl expose deployment/my-app --type="NodePort" --port 80
```
### check svc
```sh
kubectl get svc
```
### To Watch
```sh
kubectl get pods -w  
```
### Detailed info - 
```sh
kubectl describe deployment my-app
```
---
### Rollout status -
```sh
kubectl rollout status deployment/my-app 
```
### Version history -
```sh
kubectl rollout history deployment/my-app 
```
### Rollback
```sh
kubectl rollout undo deployment/my-app 
```
