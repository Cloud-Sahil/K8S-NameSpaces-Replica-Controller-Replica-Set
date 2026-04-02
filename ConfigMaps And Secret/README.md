# Kuberneties Configmap And Secret 
---
## Introduction
### What is a ConfigMap?
A **ConfigMap** is a Kubernetes resource used to store non-confidential data as key-value pairs. It helps decouple configuration data from application code.

### What is a Secret?
A **Secret** is a Kubernetes resource designed to store confidential data, such as passwords or API keys, in a secure and encoded format. Secrets are encoded using Base64, providing a layer of obfuscation but not encryption.

---
## Why Use ConfigMap and Secret?
 - **Separation of Concerns**: Decouples configuration from application logic.
 - **Ease of Updates**: Configuration changes do not require rebuilding or redeploying applications.
 - **Security**: Secrets ensure sensitive data is handled securely.
---
## Key Notes
### ConfigMap Notes
 - **Non-Confidential Data**: Do not store sensitive data.
 - **Dynamic Updates**: Can update without restarting Pods (if mounted as volume).
 - **Lightweight**: Avoid storing large or complex data.
### Secret Notes
 - **Secure Handling**: Avoid plain text files; use kubectl.
 - **Encryption**: Enable encryption at rest for better security.
 - **Access Control**: Use RBAC to restrict access.
---
## ConfigMap vs Secret in Kubernetes

###  Difference Table

| Feature | ConfigMap | Secret |
|--------|----------|--------|
| Purpose | Store non-sensitive data | Store sensitive data |
| Data Type | Plain text | Base64 encoded |
| Use Case | App configs, env variables | Passwords, tokens, API keys |
| Security Level | Low | Higher |
| Storage | Stored as plain text in etcd | Stored encoded in etcd |
| Access Control | Basic | Use RBAC for strict control |
| Command | Kubectl create configmap | Kubectl create secret |
| Example | DB host, app config | DB password, API token |

---
## 1. Create Configmap -
### Write `config.yaml`
```sh
nano config.yaml
```
```sh
apiVersion: v1 
kind: ConfigMap
metadata:
    name: my-cred
data: 
  PASSWORD: "redhat123"
  USERNAME: "admin"
  CITY: "pune"
```
### Apply Config
```sh
kubectl apply -f config.yaml
```
### Check configmap
```sh
kubectl get configmap
```
### Write `my-config.yaml`
```sh
nano my-config.yaml
```
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: app-container
          image: nginx
          envFrom:
            - configMapRef:
                name: my-configmap
```
### Apply My-config
```sh
kubectl apply -f my-config.yaml
```
### Check configmap my-cred
```sh
kubectl get configmap my-cred
```
### Describe configmap 
```sh
kubectl describe configmap my-cred
```
## 2. Create Secret -
### Write `secret.yaml`
```sh
nano secret.yaml
```
```sh
apiVersion: v1 
kind: Secret
metadata:
    name: my-secret
data: 
  PASSWORD: "cmVkaGF0"
  USERNAME: "cm9oaXQ="
  CITY: "cHVuZQ=="
```
### Apply Secret.yaml
```sh
kubectl apply -f secret.yaml
```
### Check Secret
```sh
kubectl get secret
```
### Describe secret
```sh
kubectl describe secret
```
### Check Password
```sh
echo -n "redhat" | base64
```
### Check Username
```sh
echo -n "admin" | base64
```
### Check City
```sh
echo -n "city" | base64
```
