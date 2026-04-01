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
| Security | Not secure | More secure (can enable encryption) |
| Storage | Stored as plain text in etcd | Stored encoded in etcd |
| Access Control | Basic | Use RBAC for strict control |
| Example | DB host, app config | DB password, API token |

---
## 1. Create Configmap -
### Write `config.yaml`
```sh
nano config.yaml
```
