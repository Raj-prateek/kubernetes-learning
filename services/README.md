# Services

Kubernetes Services helps us connect applications together with other applications or users.

Example our application has groups of pods running various sections such as a group for serving frontend load to users and other group for running backend process and a third group connecting to external data source.

- It is services that enable connectivity between these groups of pods.
- Services enable the frontend application to be made available to end users.
- It helps communication between backend and frontend pods.
- Helps in establishing connectivity to an external data source.

> Thus service enable loose coupling between micro services in our application.

Kubernetes Service is an object just like pods, replica set or deployments. One of its use case is to listen to a port on the node and forward request on that port to a port on the pod running the web application. This type of service is known as a node port service because the service listens to a port on the node and forward requests to the pods.

## Services Types

- **NodePort**, when the service makes an internak pod accessible on a port on the node.
  Types of Ports:
  - TargetPort, the application running port on a pod.
  - Port, the port on the service server.
  - NodePort, the port of the node

  ```
  nodeport-service-definition.yaml

  apiVersion: v1
  kind: Service
  metadata:
    name: myapp-service
  spec:
    type: NodePort
    ports:
      - targetPort: 80 # application running port (optional)
        port: 80 # service port (Mandatory)
        nodePort: 30008 
    selector:
      app: myapp
      type: front-end
  ```

- **ClusterIP**, the service that creates a virtual IP inside the cluster to enable communication between different services such as a set of front end servers to a set of backend servers. It is the default service type.

  ```
  clusterip-service-definition.yaml

  apiVersion: v1
  kind: Service
  metadata:
    name: backend
  spec:
    type: ClusterIP
    ports:
      - targetPort: 80 # application running port (optional)
        port: 80 # service exposed port (Mandatory)
    selector:
      app: myapp
      type: backend
  ```

- **LoadBalancer**, it provisions a load balancer for our application in supported cloud providers. Example to distribute load across to different web servers in your front-end teir.

  ```
  loadbalancer-service-definition.yaml

  apiVersion: v1
  kind: Service
  metadata:
    name: backend
  spec:
    type: LoadBalancer
    ports:
      - targetPort: 80 # application running port
        port: 80 # service exposed port
        nodePort: 30008
    selector:
      app: myapp
      type: backend
  ```