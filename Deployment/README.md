# Kubernetes Deployment
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
