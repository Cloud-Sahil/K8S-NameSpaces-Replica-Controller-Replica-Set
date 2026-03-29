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
