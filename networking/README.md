# Networking

When kubernetes is initially configured we create an internal private network with address 10.244.0.0 and all the pods are attach to it.
When you deploy multiple pod they all get a separate IP assigned from this network.
The pods can communicate to each other through this IP, but accessing the other pods using this internal IP address many not be a good idea as it's subject to change when pods are recreated.

## Cluster Networking

- All the containers/PODs can communicate one another without NAT.
- All nodes can communicate with all containers and vice-versa without NAT.

> Kubernetes doesn't provide any networking conflict solution by default. It expect us to setup a networking solution that can prevent the conflicts.

Tools which can help in setup this conflicts are flannel, VM ware NSX, cisco, cilium etc.
