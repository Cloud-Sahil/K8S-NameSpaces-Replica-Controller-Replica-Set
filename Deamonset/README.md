# Kubernetes Deamonset
---
## A DaemonSet automatically deploys a copy of a Pod on all (or selected) nodes.
### Features of DaemonSets

 - Runs one pod per node.
 - Automatically schedules pods on newly added nodes.
 - Ensures uninterrupted system monitoring and logging.

---
```sh
apiVersion: apps/v1
kind:  DaemonSet
metadata: 
  name: daemon
  labels:
    app: depapp
spec:
    selector: 
      matchLabels: 
          app: depapp
    template:
      metadata:
        labels:
          name: cwatch
          app: depapp
      spec:
        containers:
          - name: cwatch
            image: amazon/cloudwatch-agent:latest
            ports:
                - containerPort: 80
                  protocol: TCP
```

```sh
apiVersion: apps/v1
kind: DaemonSet
metadata: 
    name: my-dmnset
spec: 
    selector:
      matchLabels:
          app: fluentd
    template:
      metadata:
        labels:
          app: fluentd
      spec:
        nodeSelector:
            hostname: node01
        containers:
          - name: fluentd
            image: fluentd:latest
            ports:
              - containerPort: 24224
```
