# Pods vs Deployments in K8s

### Pods

- A pod is the smallest unit in kubernetes
- It represents a single instance of process running in a cluster which may consist of one or more than one  containers that may share the same network namespace
- Pods have short lifecycle and are considered ephemeral . If a pod fails or terminated , Kubernetes will not attempt to recover it .

### Deployments

- A deployment is a higher level resource that manages the desired state for a set of replica pods
- Deployment offers features such as Auto Healing and Auto-Scaling . 
- It allows user to define the number of desired replicas (copies) of the Pods to be maintained at all times. If a Pod goes down, the Deployment controller will automatically create a new one to replace it.


### Replica Set

Whenever a Deployment is created , it will create a replicaset for the user , Which is responsible for maintaining the desired state of the deployment i.e. autohealing . 


### How to deploy a deployment in K8s

To deploy an application using a Deployment in Kubernetes, you'll need to create a YAML manifest file 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp-container
          image: your-container-image:tag
          ports:
            - containerPort: 80

```

- Apply the deployment to your kubernetes cluster

```
kubectl apply -f myapp-deployment.yaml

```

- To verify the deployments

```
kubectl get deployments

```

You will also be able to see that the replicaset which acts as the controller for the deplyment will automatically be created which can be verified by the following command

```
kubectl get rs
```
The Required number of pods which are mentioned in the Yaml manifest file will also be created and maintained by the replicaset controller

```
kubectl get pods
```

## Conclusion

Deployments provide a higher level of abstraction and bring benefits like fault tolerance, scalability, and rolling updates to your application, making it easier to manage and maintain in a Kubernetes cluster. It's common to use Deployments for long-running services and Pods for temporary batch jobs or containers that need to be colocated on the same node.