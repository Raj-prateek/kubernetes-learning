# ReplicaSets Controllers

Let's go back to first scenario where we had a single pod running our application. What if for some reason our application crashes and the pod fails? Users will no longer be able to access our application. To prevent users from losing access to our application. We want to more than one instance or pod running at the same time. That way, if one fails, we still have our application running on the other one.

The replication controller helps us run multiple instances of a single pod in the kubernetes cluster, thus providing high availability.

So does that mean you can't use a replication controller if you plan to have a single pod? No, even if you have a single pod,

- The replication controller can help by automatically bringing up a new pod when the existing one fails.
- The replication controller ensures that the specified number of pods are running at all times, even if it's just one or hundred.

## Load Balancing & Scaling

- Replication controller helps in sharing the load across them.
- Replication controller spans across multiple nodes in the cluster.
- It helps us balance the load across multiple pods on different nodes as well as scale our application when the demand increases.

## Replication Controller VS Replica Set

- Replication controller is the older technology that is being replaced by replica set.
- Replica set is the new recommended way to set up replication.

> There are minor difference in the way each works.

```
rc-definition.yaml

apiVersion: v1
kind: ReplicationController
metadata: # Replica Controller
  name: myapp-rc
  labels:
    app: myapp
    type: front-end
spec: # Replica Controller
  template:
    metadata: # Pod
      name: frontend-app
      labels:
          app: app
          type: frontend
    spec: # Pod
      containers:
        - name: nginx-container
          image: nginx
  replicas: 3
```

Run command `kubectl create -f rc-definition.yaml` to create replica controller.

To get list of controller run command `kubectl get replicationcontroller`


```
replicaset-definition.yaml


apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
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
        - name: nginx
          image: nginx
  replicas: 3
  selector: # required
    matchLabels:
      type: front-end
```

Run command `kubectl create -f replicaset-definition.yaml` to create replica controller.

To get list of controller run command `kubectl get replicaset`

### Label & Selectors

Let us understand using an example, say we deployed three instances of our front-end web application as three pods. We would like to create a replication controller or replica set to ensure that we have three active pods are any time. That is one of the use case of replica sets. You can use it to monitor existing pods if you have them already created. In case they were not created, the replica set will create them for you.

- The role of the replica set is to monitor the pods and if any of them were to fail, deploy new ones.
- The replica set is in fact a process that monitors the pods.

Labeling pods during creation comes into account in n number of pods. Labels act as a filter for replicaset set under the selection section. Under the selector section, we use to match labels, filter and provide the same label that we used while creating the pods. This way the replica set knows which part to monitor.

The same concept of labels and selectors is used in many other places throughout kubernetes.

### Scale

- `kubectl replace -f replicaset-definition.yaml`
- `kubectl scale --replicas=6 -f replicaset-definition.yaml`
- `kubectl scale --replicas=6 replicaset myapp-replicaset`

### Commands

> `kubectl create -f replicaset-definition.yaml`
> `kubectl get replicaset`
> `kubectl delete replicaset myapp-replicaset` # Also deletes all underlying PODs
