# Pods

## Assumptions

- Docker Image is already setup. We will be using [beginner-node-app](https://hub.docker.com/repository/docker/prateek9958/beginner-node-app).
- Kubernetes Cluster has already been setup and it is working.

As we discussed earlier that our ultimate aim is to deploy our application in the form of containers on a set of machines that are configured as worker nodes in a cluster.
However, it does not deploy containers directly on the worker nodes.

- The containers are encapsulated into a commonalties object known as pods.
- A pod is a single instance of an application.
- A pod is the smallest object that you can create in kubernetes cluster.
- Pods can have one to one relationship in the container running application to scale up.
- To scale up you add pod and to delete an existing pod.

Let us create nginx pod `kubectl run nginx --image=nginx`

## Commands

- To get the list of pods in the cluster run command:
  `kubectl get pods`
  Output:

  ```
  NAME    READY   STATUS              RESTARTS   AGE
  nginx   0/1     ContainerCreating   0          14s
  ```

  ```
  NAME    READY   STATUS    RESTARTS   AGE
  nginx   1/1     Running   0          16s
  ```

  **Options**
  - `wide` this provide addition information such as the node where the pod is running and the IP address of the pod as well.

  ```
    NAME    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
    nginx   1/1     Running   0          43m   172.17.0.3   minikube   <none>           <none>
  ```

- To get pod full information use:
  `kubectl describe pod nginx`

  ```
  Name:         nginx
  Namespace:    default
  Priority:     0
  Node:         minikube/192.168.64.2
  Start Time:   Thu, 08 Jul 2021 21:32:47 +0530
  Labels:       run=nginx
  Annotations:  <none>
  Status:       Running
  IP:           172.17.0.3
  IPs:
    IP:  172.17.0.3
  Containers:
    nginx:
      Container ID:   docker://a8e556c3c2747caffce5bd0e857c3f5245dd957393974541c27da67ab96a5940
      Image:          nginx
      Image ID:       docker-pullable://nginx@sha256:3ca76089b14cf7db77cc5d4f3e9c9eb73768b9c85a0eabde1046435a6aa41c06
      Port:           <none>
      Host Port:      <none>
      State:          Running
        Started:      Thu, 08 Jul 2021 21:33:01 +0530
      Ready:          True
      Restart Count:  0
      Environment:    <none>
      Mounts:
        /var/run/secrets/kubernetes.io/serviceaccount from default-token-r74kz (ro)
  Conditions:
    Type              Status
    Initialized       True 
    Ready             True 
    ContainersReady   True 
    PodScheduled      True 
  Volumes:
    default-token-r74kz:
      Type:        Secret (a volume populated by a Secret)
      SecretName:  default-token-r74kz
      Optional:    false
  QoS Class:       BestEffort
  Node-Selectors:  <none>
  Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                  node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
  Events:
    Type    Reason     Age   From               Message
    ----    ------     ----  ----               -------
    Normal  Scheduled  22s   default-scheduler  Successfully assigned default/nginx to minikube
    Normal  Pulling    22s   kubelet            Pulling image "nginx"
    Normal  Pulled     9s    kubelet            Successfully pulled image "nginx" in 12.39363458s
    Normal  Created    9s    kubelet            Created container nginx
    Normal  Started    9s    kubelet            Started container nginx
  ```

- To edit pod configurations
  `kubectl edit pod podname`

- To edit pod configurations using custom file and apply.
  `kubectl apply -f file.yaml`

- To write the pod configurations using run command.
  `kubectl run redis --image=redis123 --dry-run=client -o yaml > pod.yaml`

References:

[Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

## PODs with YAML

[**pod-definition.yaml**](/pod-definition.yaml)

```
apiVersion: v1
kind: Pod
metadata:
  name: frontend-app
  labels:
      app: app
      type: frontend
spec:
  containers:
    - name: nginx-container
      image: nginx
```

Run command `kubectl create -f pod-definition.yaml` to create a pod.

```
kubectl get pods

NAME           READY   STATUS    RESTARTS   AGE
frontend-app   1/1     Running   0          16s
```

```
kubectl describe pod frontend-app

Name:         frontend-app
Namespace:    default
Priority:     0
Node:         minikube/192.168.64.2
Start Time:   Fri, 09 Jul 2021 00:23:01 +0530
Labels:       app=app
              type=frontend
Annotations:  <none>
Status:       Running
IP:           172.17.0.4
IPs:
  IP:  172.17.0.4
Containers:
  nginx-container:
    Container ID:   docker://fc70a4816ed9774a2b9109debe6f9912c0b0f10e801ee65dce64676b8cbf4c62
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:3ca76089b14cf7db77cc5d4f3e9c9eb73768b9c85a0eabde1046435a6aa41c06
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Fri, 09 Jul 2021 00:23:06 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-r74kz (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-r74kz:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-r74kz
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  3m46s  default-scheduler  Successfully assigned default/frontend-app to minikube
  Normal  Pulling    3m46s  kubelet            Pulling image "nginx"
  Normal  Pulled     3m42s  kubelet            Successfully pulled image "nginx" in 3.480545408s
  Normal  Created    3m42s  kubelet            Created container nginx-container
  Normal  Started    3m42s  kubelet            Started container nginx-container
```

