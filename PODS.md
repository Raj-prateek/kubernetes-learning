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

References:

[Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
