# Deployment

Kubernetes object that comes higher in the hierarchy, the deployment provides us with the capability to upgrade the underlying instances seamlessly using rolling updates. Undo changes and pause and resume changes as required.


```
deployment-definition.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx
  replicas: 3
  selector: 
    matchLabels:
      type: front-end
```

Run command `kubectl create -f deployment-definition.yaml` to create deployment.

`kubectl get deployments` to get all the deployment lists.

## Commands

> `kubectl get all`
