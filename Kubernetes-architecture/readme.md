# Kubernetes

### Problems in docker 

- Docker is based on single host
- Ephemeral in nature 
- No autohealing 
- No autoscaling
- Does not provide enterprise level standards/support
    

The Above problems can be solved by using kubernetes

- Kubernetes follows the **cluster** approach by default
- it also consists of replication controller that is used for **auto scaling**
also supports HPA - horizontal pod **autoscaler**
- kubernetes controls and fixes the damage i.e. autohealing
- Kubernetes is **enterprise level ready** which supports the features like firewall ,load balancers ,whitelisting,blacklisting ,autoscaling etc
- Kuberenetes has **strong CNCF community** and is improving everyday

![basic overview of kubernetes architecture](https://cdn.hashnode.com/res/hashnode/image/upload/v1681929546257/92239821-aac2-44db-b7c1-1528c8cf8dcd.png)

## Components present in Kubernetes architecture

### Control Plane/Master Node 

- **API server** : API server is used for communication of external kubernetes-service-implementation-with-kubshark0world and kubernetes .External communications via command line interface (CLI) or other user interfaces (UI) pass to the kube-apiserver, and all control planes to node communications also goes through the API server.

- **Scheduler** : A scheduler watches for newly created Pods that have no Node assigned. For every Pod that the scheduler discovers, the scheduler becomes responsible for finding the best Node for that Pod to run on. 

- **etcd** : IT store all the data related to cluster in a key value pair form. Information is generally stored in human readable YAML format .

- **controller-manager** : a controller is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state.

- **cloud controller-manager** : The cloud-controller-manager is a Kubernetes control plane component that embeds cloud-specific control logic. The cloud controller manager lets you link your cluster into your cloud provider's API, and separates out the components that interact with that cloud platform from components that only interact with your cluster.


### Worker Node / Data plane 

- **kubelet** : Every node has an agent called kubelet. It ensures that the container described in PodSpecs are up and running properly. 

- **kube-proxy** :  A network proxy on each node that maintains network nodes which allows for the communication from Pods to network sessions, both inside or outside the cluster

- **container runtime** : for the container to run we need container runtime . in kubernetes docker is not mandatory therefore we can use dockershim , containerd, cri-o or another container runtimes which implements kubernetes container interface .Software responsible for running the containerized applications. Although Docker is the most popular, Kubernetes supports any runtime that adheres to the Kubernetes CRI (Container Runtime Interface).
