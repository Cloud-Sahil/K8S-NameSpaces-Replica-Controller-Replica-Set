# Kuberneties Persistent Volume(PV) And Persistent Volume Claim(PVC)
---
## Introduction
### Persistent Volume (PV)
A **Persistent Volume** is a storage resource in a Kubernetes cluster that provides persistent storage, independent of Pod lifecycles. It is defined and managed by the cluster administrator.

### Persistent Volume Claim (PVC)
A **Persistent Volume Claim** is a request for storage by a user. Pods use PVCs to access PVs.

### Dynamic Provisioning
Dynamic provisioning automatically creates PVs based on a PVC when a StorageClass is specified. This is particularly useful for cloud-based storage systems like AWS EBS.

---
## Commands -
### Write `pv.yaml`
```sh
nano pv.yaml
```
```sh
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-example
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: "/mnt/data"
```
### Write `pvc.yaml`
```sh
nano pvc.yaml
```
```sh
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-example
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: manual
```
### Write `pod.yaml`
```sh
nano pod.yaml
```
```sh
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: myfrontend
    image: nginx
    volumeMounts:
    - mountPath: "/var/www/html"
      name: pv-example

  volumes:
  - name: pv-example
    persistentVolumeClaim:
      claimName: pvc-example
```
### Apply pv
```sh
kubectl apply -f pv.yaml
```
### Apply pvc
```sh
kubectl apply -f pvc.yaml
```
### Apply pod
```sh
kubectl apply -f pod.yaml
```
### Check Resources pv
```sh
kubectl get pv
```
### Check Resources pvc
```sh
kubectl get pvc
```
### Check Resources pod
```sh
kubectl get pods
```
### Storage path PV
```sh
mkdir -p /mnt/data
```
### Describe Pods
```sh
kubectl describe pods
```
### Describe PV
```sh
kubectl describe pv
```
### Describe PVC
```sh
kubectl describe pvc
```
### Describe Pod `mypod`
```sh
kubectl describe pod mypod
```
### Access Pod
```sh
kubectl exec -it mypod -- sh
```
```sh
echo "Hello World" > /usr/share/nginx/html/index.html
```
```sh
exit
```
### Pods Details
```sh
kubectl get pods -o wide
```
### Testing Application
```sh
curl `<ip>`
```
### Delete pod
```sh
kubectl delete pod mypod
```
### Verify Deletion
```sh
kubectl get pods
```
### Check Persistent Data - (Go to PV storage directory)
```sh
cd /mnt/data
```
### List Files
```sh
ls
```
### View file content - (Confirms data persistence after pod deletion)
```sh
cat index.html
```
### Exit
```sh
cd
```
---
### Recreate Pod - `Metadata name is change`
```sh
nano pod.yaml
```
```sh
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: myfrontend
    image: nginx
    volumeMounts:
    - mountPath: "/var/www/html"
      name: pv-example

  volumes:
  - name: pv-example
    persistentVolumeClaim:
      claimName: pvc-example
```
### Recreate Pod Apply 
```sh
kubectl apply -f pod.yaml
```
### Check Pod
```sh
kubectl get pods
```
### Check All pods
```sh
kubectl get pods -o wide
```
### Test Again
```sh
curl `<ip>`
```
---
### Check Disk Space Usage
```sh
df -hT
```
