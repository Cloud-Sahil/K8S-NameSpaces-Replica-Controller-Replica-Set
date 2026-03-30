# Kubernetes StatefulSet 

## StatefulSet is used to manage **stateful applications** in Kubernetes.

- Provides **unique identity** to each pod
- Maintains **stable hostname and storage**
- Pods are created and deleted **in order (sequentially)**
---
## Key Features
 - **Stable Pod Names** → pod-0, pod-1, pod-2 (fixed identity)

 - **Persistent Storage** → Each pod gets its own PersistentVolume
→ Data is not lost after restart

 - **Ordered Deployment** → Pods start one by one (0 → N)
→ Termination happens in reverse order

 - **Stable Network Identity** → Each pod has a fixed DNS name

---
## Use Cases
 - Databases (MySQL, MongoDB)
 - Distributed systems (Kafka, Zookeeper)
 - Applications requiring persistent data
---
### Write yaml file
```sh
nano database.yaml
```
```sh
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-stateful-app
spec:
  serviceName: "my-service"
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
        volumeMounts:
        - name: data
          mountPath: /data

  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```
```sh
apiVersion: apps/v1
kind: StatefulSet
metadata: 
    name: my-sts
spec:
  selector: 
    matchLabels: 
        app: my-app
  serviceName: "my-sts-service"
  replicas: 3
  template:
    metadata:
      labels:
        app: my-app
    spec:
        containers:
           - name: mysql
             image: mysql:latest
             env:
              - name: MYSQL_ROOT_PASSWORD    
                value: redhat
             ports:
                - containerPort: 3306
```
### Apply file
```sh
kubectl apply -f database.yaml
```
### Check pods
```sh
kubectl get pods
```
### Delete pods
```sh
kubectl delete pod <name>
```
