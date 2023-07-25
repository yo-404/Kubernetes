# Deploying first app on Kubernetes 

### install kubectl on host

For x86-64 Linux

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

For ARM64 Linux
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

if using any other OS/distro

vist https://kubernetes.io/docs/tasks/tools/

to check whether your kubectl is installed or not use this command -

```
kubectl version 
```

```
kubectl get nodes
vi pod.yml
```
Paste the following pod template for nginx 

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

```

the equivalent command for the above yaml file in docker would have been

```
docker run -d nginx:1.14.2 --name nginx -p 80:80
```

------------------------------------------------------------

### to create the pod from the yaml file 

use command 
```
kubectl create -f pod.yml
```

### to check if the pod is running 

```
kubectl get pods
```

to get more details about the pods like IP ,Node etc - use

```
kubectl get pods -o wide
```
apart from that this command can also be used to get details in yaml format

```
kubectl describe pods <pod name>
```
### to login into the cluster

- ssh into the master node ip address and curl into the ip address

```
curl 172.17.0.3
```

if using minikube - ssh into minikube

```
minikube ssh
```

### for deleting a pod

```
kubectl delete pod <pod-name>
```

### to check logs for pod

```
kubectl logs <pod-name>
```

### Some other basic commands
```
kubectl get deployments
kubectl get nodes
kubectl get pods

```