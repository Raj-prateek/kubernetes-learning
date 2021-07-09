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

## Rollout & Versioning

When you first create a deployment, it triggers a rollout, a new rollout creates a new deployment revision.

Lets say we have rollout revision 1. In the future, when the application is upgraded, meaning when the container version is updated to a new one, a new rollout is triggered and a new deployment revision is created named revision 2.
This helps us keep track of the changes made to our deployment and enables us to roll back to a previous version of deployment if necessary.

### Commands

> `kubectl rollout status deployment/myapp-deployment`

> `kubectl rollout history deployment` will show you the revision and history of our deployment.

### Deployment Strategy

There are two types of deployment strategies, say, for example you have replicas of your web application instance deployed.

- One way to upgrade these to a newer version is to destroy all of these and then create newer versions of application instances, meaning first destroy the five running instances and then deploy five running instances of the new application version. The problem with this, as you can imagine, is that during the period after the older versions down and before any newer version is up, the application is down and inaccessible to users. This strategy is known as the `recreate strategy` and thankfully this is not the default deployment startegy.

- The second strategy is where we did not destory all of them at once. Instead, we take down the older version and bring up a newer version, one by one. This way, the application never goes down and the upgrade is seamless. By default deployment strategy is a `rolling update`.

#### Recreate VS Rolling Update

When recreate strategy was used, the events indicate that the old replicaset was scaled down to zero first and then the new replica sets scaled up to five.

However, when the rolling update strategy was used, the old replica set was scaled down one at a time, simultaneously scaling up the new replica set, one at a time.

### Upgrades

- It first creates a replica set automatically, which in turn creates the number of pods required to meet the number of replicas.
- When you upgrade your application , the kubernetes deployment object creates a new replica set under the hood and starts deploying the containers there.
- At the same time, taking down the pods in the old replica set following a rolling update strategy.

### Rollback

Say for instance, once you upgrade your application, you realize something isn't very right, something is wrong the new version of build you used to upgrade. So you would like to roll back your update.
Kubernetes deployment allow you to roll back to a previous revision to undo a change, run the `kubectl rollout undo deployment/myapp-deployment`. The deployment will then destroy the part in the new replica set and bring the older ones up in the old replica set and the application is back to it older format.
