# Kubernetes Relication Controller
---
## Old way of managing Pods.

### Key points:
 - Ensures specified number of Pod replicas
 - Uses **equality-based selectors only**
 - **Deprecated** (replaced by ReplicaSet)
---
## Write rc Yaml File 
~~~sh
nano rc.yaml
~~~
~~~sh
apiVersion: v1
kind: ReplicationController
metadata:
  name: test-rc
  namespace: test
spec:
  replicas: 3
  selector:
    app: test-app
  template:
    metadata:
      labels:
        app: test-app
    spec:
      containers:
      - name: test-container
        image: nginx
        ports:
        - containerPort: 80
~~~
## Apply rc yaml file
~~~sh
kubectl apply -f rc.yaml
~~~
## Check rc
~~~sh
kubectl get rc -n test
~~~
## check pods namespace
~~~sh
kubectl get pods -n test
~~~
## delete rc
~~~sh
kubectl delete pod test -rc
~~~
