# Kubernetes HPA With Minikube
---
## EC2 
 - Instance Size: t2.large, etc  with 2 CPUs
 - 32 GB Storage <-- volume_size
## Install kubectl
Download the latest release with the command:
````
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
````

Install kubectl:
````
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
````
- use only when you did not have and root acess (sudo) on EC2 
````
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
````
````
kubectl version --client    # to check version 
````
## Install Minikube
````
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
````
````
sudo install minikube-linux-amd64 /usr/local/bin/minikube
````
````
minikube start
````
---

## Create Deployment 
```bash
vim hpa-dep.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hpa-nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hpa-nginx
  template:
    metadata:
      labels:
        app: hpa-nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        resources:
          requests:
            cpu: "100m"
          limits:
            cpu: "200m"
        ports:
        - containerPort: 80
```
### Apply HPA.Deployment 
```bash
kubectl apply -f hpa-dep.yaml
```

---
## Create Service.yaml
```bash
vim service.yaml
```
```yaml
apiVersion: v1
kind: Service
metadata:
  name: hpa-service
spec:
  selector:
    app: hpa-nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```
### Apply Svc
```bash
kubectl apply -f  service.yaml
```
---
## Create Hpa.yaml
```bash
vim hpa.yaml
```

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-nginx
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: hpa-nginx
  minReplicas: 1
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 20
```
### Apply HPA
```bash
kubectl apply -f hpa.yaml
```
---
## Generate Load
```bash
kubectl run -i --tty load-generator --image=busybox /bin/sh
```
*note:* - run below command in shell

```bash
while true; do wget -q -O- http://hpa-service; done
```
## check
- take an ssh on another terminal to check the cluster pods and hpa 
```bash
kubectl get hpa -w
kubectl get pods
```
---
## Stop Minikube
```bash
minikube stop
```


