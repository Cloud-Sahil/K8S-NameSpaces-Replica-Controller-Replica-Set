# Kubernetes Persistent Volume(PV) And Persistent Volume Claim(PVC)
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
--- 

## **1. Persistent Volume (PV)**
### **Example: Persistent Volume (PV)**
```yaml
apiVersion: v1   # API version used for PersistentVolume resource
kind: PersistentVolume   # Declares this manifest as a PersistentVolume (PV)
metadata: 
    name: my-vol   # Name of the PV (unique identifier in the cluster)

spec: 
  capacity: 
    storage: 5Gi   # The size of the PV (5 Gigabytes)

  volumeMode: Filesystem   # Specifies that the volume will be mounted as a filesystem (not raw block)

  accessModes:
      - ReadWriteOnce   # Only one node can mount this volume for read/write at a time

  persistentVolumeReclaimPolicy: Retain   # What happens when the PVC is deleted:
                                          # Retain → Keep the data
                                          # Delete → Remove the PV and data
                                          # Recycle → Wipe and make PV available again (deprecated)

  storageClassName: admin   # StorageClass name associated with this PV (used for dynamic provisioning)

  local:
    path: /mnt/data   # Path on the node’s local filesystem where data will be stored

  nodeAffinity:   # Ensures the PV is only usable on specific nodes
    required:
      nodeSelectorTerms: 
        - matchExpressions:
            - key: kubernetes.io/hostname   # Node label key used for selection
              operator: In                  # Operator meaning "must be in this list"
              values: ["controlplane"]      # Only the node with hostname "controlplane" can use this PV

```

--- 

## For NFS (Network File System)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-1
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  nfs:
    path: /tmp
    server: 172.17.0.2
```
---

## For EBS (Elastic Block Store)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-1
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  awsElasticBlockStore:
    volumeID: "<volume id>"
    fsType: ext

```


 
 ## 2. Persistent Volume Claim (PVC)

 ```yaml
apiVersion: v1   # API version for PersistentVolumeClaim resource
kind: PersistentVolumeClaim   # Declares this manifest as a PVC
metadata: 
    name: my-pvc   # Name of the PVC (unique identifier in the namespace)

spec: 
  accessModes:
    - ReadWriteOnce   # Access mode: the PVC requires a volume that can be mounted read/write by only one node

  resources:
    requests:
      storage: 5Gi   # The PVC is requesting 5Gi of storage

  storageClassName: admin   # Must match the PV's storageClassName so that it binds correctly

  volumeName: my-vol   # Explicitly binds this PVC to the PersistentVolume named "my-vol"

```


## 3. Pod Using PVC
```yaml
apiVersion: v1   # API version for Pod resource
kind: Pod        # Declares this manifest as a Pod
metadata: 
  name: my-pod   # Name of the Pod

spec:
  containers:
    - name: nginx   # Container name inside the Pod
      image: nginx:latest   # Container image to run (latest Nginx)
      ports:
        - containerPort: 80   # Exposes port 80 (default HTTP port) inside the container

      volumeMounts:   # Defines where to mount volumes inside the container
        - mountPath: "/usr/share/nginx/html"   # Path inside container where volume will be mounted
          name: my-vol   # Must match the volume name defined below

  volumes:   # Volumes available to the Pod
    - name: my-vol   # Name of the volume (referenced above in volumeMounts)
      persistentVolumeClaim:
        claimName: my-pvc   # PVC name → binds this volume to the PVC "my-pvc"
 
```
