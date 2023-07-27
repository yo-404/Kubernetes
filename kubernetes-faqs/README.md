# Kubernetes FAQs

### What is the difference between docker and kubernetes

Docker is an open source containerization platform whereas kubernetes is an container orchestration platform . Additionally kubernetes also offers features like Auto-Healing , Auto Scaling , Clustering and Enterprise level support like Load balancing . Docker works on single Node whereas kubernetes works on cluster .


### What are the main components of kubernetes architecture

#### Control Plane/Master Node 

- **API server** : API server is used for communication of external world and kubernetes .External communications via command line interface (CLI) or other user interfaces (UI) pass to the kube-apiserver, and all control planes to node communications also goes through the API server.

- **Scheduler** : A scheduler watches for newly created Pods that have no Node assigned. For every Pod that the scheduler discovers, the scheduler becomes responsible for finding the best Node for that Pod to run on. 

- **etcd** : IT store all the data related to cluster in a key value pair form. Information is generally stored in human readable YAML format .

- **controller-manager** : a controller is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state.

- **cloud controller-manager** : The cloud-controller-manager is a Kubernetes control plane component that embeds cloud-specific control logic. The cloud controller manager lets you link your cluster into your cloud provider's API, and separates out the components that interact with that cloud platform from components that only interact with your cluster.


#### Worker Node / Data plane 

- **kubelet** : Every node has an agent called kubelet. It ensures that the container described in PodSpecs are up and running properly. 

- **kube-proxy** :  A network proxy on each node that maintains network nodes which allows for the communication from Pods to network sessions, both inside or outside the cluster

- **container runtime** : for the container to run we need container runtime . in kubernetes docker is not mandatory therefore we can use dockershim , containerd, cri-o or another container runtimes which implements kubernetes container interface .Software responsible for running the containerized applications. Although Docker is the most popular, Kubernetes supports any runtime that adheres to the Kubernetes CRI (Container Runtime Interface).


### What are the main differences between Docker swarm and kubernetes 

- Docker Swarm is maintained by docker inc. whereas kubernetes was originally developed by Google and later donated to CNCF ( Cloud native computing foundation) 

- Docker Swarm follows a simpler architecture compared to kubernetes.Docker swarm architecture consists of manager node and worker node where manager handles the orchestration and worker node runs the container . communication between these nodes is done by gossip protocol or via a key value store
In kubernetes the arhitecture have multiple components including an API Server , scheduler , controller manager , etdc . Node in kubernetes include a kubelet ,kubeproxy and container run time .

- Docker swarm comes with essential orhcestration features that overs most of the need of the user with a straightforward approach
Whereas in kubernetes the user get more control over the containerization application . It supports advanced deployments strategies like canary , blue/green .Also provides more flexible networking options and custom resource definitions (CRDs)

- Docker swarm has a smaller community when compared to kubernetes 

- **use case**
    Docker Swarm: Docker Swarm is well-suited for simpler, smaller-scale container orchestration needs or for users who prefer a more straightforward setup and already use Docker in their development workflow.
    Kubernetes: Kubernetes shines when it comes to complex, large-scale deployments, multi-cloud and hybrid cloud environments, and enterprise-grade production workloads. It offers more extensive capabilities for managing highly available, resilient, and scalable containerized applications.


### What is difference between docker container and kubernetes pod

A pod in kubernetes is a runtime specification of a container in docker . A pod provides more declarative way of definition using YAML and user can run more than one container in a pod

Docker containers are typically used for running individual microservices or applications in isolation, where each container represents a single running process.

### what is a namespace in Kubernetes

In Kubernetes, namespace is a logical isolation of resource ,network policies ,**rbac** and everything . For e.g.- there are two projects using the same kubernetes cluster . One project can use ns1 and other project can use ns2 without any overlap and authentication problems 

rbac -RBAC stands for Role-Based Access Control, and it is a security mechanism used to manage and control access to resources in a system or application. RBAC is widely used in various software systems, including operating systems, databases, and cloud platforms, to enforce permissions and restrictions on users or entities based on their roles or responsibilities within the organization.
In Kubernetes , RBAC is used to control access to various resources and actions within a Kubernetes cluster. It enables administrators to define fine-grained access policies and grant specific permissions to users, groups, or service accounts based on their roles


### Role of kubeproxy 

Kube-proxy is an essential component of a kubernetes cluster , as it ensures that services can communicate with each other .Kube-proxy is a critical component in a Kubernetes cluster responsible for handling network communications between services and ensuring that network traffic is correctly routed to the appropriate Pods. It is part of the Kubernetes control plane and runs on each node in the cluster.

Kube-proxy works by maintaining a  set of network rules on each node in the cluster , which are updated dynamically as services are added or removed . When a client sends a request to sa service  , the requires is intercepted by kube-proxy on the node where it was recieved . Kube-proxy then looks up the destination endpoint for the service and routes the request accordingly

### what are the different types of services within kubernetes

- **ClusterIP**: The default type. It exposes the Service on a cluster-internal IP address, and it's only accessible within the cluster.

- **NodePort**: Exposes the Service on each Node's IP address at a static port. It opens a high port on all cluster Nodes and forwards traffic to the Service.

- **LoadBalancer**: Requests an external load balancer from the cloud provider to route external traffic to the Service.

- **ExternalName**: Maps the Service to an external DNS name without creating any virtual IP addresses or endpoints within the cluster. It's useful when you want to direct traffic to an external service outside the cluster.


### What is the difference between NodePort and LoadBalancer type service

When a service is created a NodePort type, The kube-proxy updates the IPTables with Node IP address and port that is chosen in the service configuration to access the pods.

Whereas if you create a Service as type LoadBalancer, the cloud control manager creates a external load balancer IP using the underlying cloud provider logic in the C-CM (Cloud controller manager). Here users can access the services using the external IPs


### What is the role of Kubelet

kubelet manages the containers that are scheduled to run on that node . It ensures that the container are running and are healthy ,and that the resources they need are available .

Kubelete communicates with the kubernetes API server to get information about the container that should be running on the node and then starts and stops the containers as needed to maintain the desired state . It also monitors the containers to ensure that they are running correctly,  and restarts them if necessary