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
### Write `pvc.yaml`
```sh
nano pvc.yaml
```
### Write `pod.yaml`
```sh
nano pod.yaml
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
