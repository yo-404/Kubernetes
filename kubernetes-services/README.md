# Kubernetes service

- In Kubernetes, a Service is an abstraction that defines a logical set of Pods and a policy by which to access them. 
- It provides a stable endpoint (IP address and DNS name) for other components within and outside the cluster to interact with the Pods running your application

### Some features that kuberenets service provides

- Load balancing
- Service Discovery
- Exposing application to the external world

### labels and selectors

Since pods are ephemeral in nature and despite of being deployed as a deployment ,it will be able to heal itself but the provided IP for the pods will change every time the the pod is recreated . The counter this issue kubernetes uses something known as service .It helps in service discovery mechanism .It discovers the related pods with the help of labels and selectors. In the declarative YAML manifest user must provide a label for the deployment . The service then uses the labels in the pods to identify the related pods .


### Service Types 

- **ClusterIP**: The default type. It exposes the Service on a cluster-internal IP address, and it's only accessible within the cluster.

- **NodePort**: Exposes the Service on each Node's IP address at a static port. It opens a high port on all cluster Nodes and forwards traffic to the Service.

- **LoadBalancer**: Requests an external load balancer from the cloud provider to route external traffic to the Service.

- **ExternalName**: Maps the Service to an external DNS name without creating any virtual IP addresses or endpoints within the cluster. It's useful when you want to direct traffic to an external service outside the cluster.


### Benefits of kubernetes service

- Load Balancing: Kubernetes Services provide a stable and unique DNS name and IP address for a set of pods. This allows other components within the cluster to easily discover and communicate with the service without having to know the specific details of individual pod IPs.

- Service discovery: By defining multiple replicas of a pod and exposing them through a Service, Kubernetes provides a highly available architecture. If a pod becomes unavailable or goes down, the Service will automatically route traffic to the remaining healthy pods.

- High Availabilty: Services allow for easy horizontal scaling of applications. You can adjust the number of replicas in a Deployment, and Kubernetes will automatically manage the scaling of the underlying pods, making it simple to handle changes in traffic demands.

- Scaling: Services enable application decoupling from the underlying infrastructure. The Service abstraction allows you to modify the underlying pods or change their placement without affecting the consumers of the service.

- Application Decoupling : Kubernetes Services can be exposed both internally (within the cluster) and externally (outside the cluster). ClusterIP type Services provide internal connectivity, while NodePort or LoadBalancer types allow external access to the services.
- Easy upgrades and rollback : Kubernetes supports rolling updates, which allow you to update the underlying pods of a Service without any downtime. This ensures that your application remains available during updates, making it easier to perform upgrades and rollbacks.

- Integration with Load Balancer: When using cloud providers, Kubernetes Services can be integrated with cloud load balancers, which can further enhance the load balancing capabilities and provide external access to the services.

- Affinity and Anti-Affinity: Services support affinity and anti-affinity configurations that allow you to influence the scheduling of pods. This can be useful for deploying pods on specific nodes or avoiding co-location of certain pods.

- Circuit Breaker Pattern: With features like readiness and liveness probes, Kubernetes Services can implement the circuit breaker pattern, automatically removing unhealthy pods from the service endpoint until they become healthy again.