# Kubernetes services implementation with kubeshark traffic monitoring 

locate your project file which contains the Dockerfile to create the image and all the dependencies listed in requirements.txt 

In our case , we are using python-web-app

```
docker build -t yogi404/python-sample-app-demo:v1 .
```

Now , we will create a deployment .yaml file for deploying the created image from the above step

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-py-app
  labels:
    app: sample-py-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: sample-py-app
  template:
    metadata:
      labels:
        app: sample-py-app
    spec:
      containers:
      - name: py-app-sample
        image: yogi404/python-sample-app-demo:v1
        ports:
        - containerPort: 8000


```

```
kubectl apply -f deployment.yml
```
```
kubectl get deploy
kubectl get pods
```

to get the ip addresses of the pods 
```
kubectl get pods -o wide
```

Since pods are ephemeral in nature and when one pod goes down and the new one comes up , the IP address for the newly generated pod will be different from the older pod. To identify the pod ,we use labels and selectors . pods for the deployment will be identified using the label provided in the yaml manifest file . 

## Creating service 

service.yml

```
apiVersion: v1
kind: Service
metadata:
  name: py-django-app-sv
spec:
  type: NodePort
  selector:
    app: sample-py-app
  ports:
    - port: 80
      targetPort: 8000
      # Optional field
      # By default and for convenience, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
      nodePort: 30007
```
make sure to copy the exact name of selector from the deployment.yaml label section

```
kubectl apply -f service.yml
```

```
kubectl get svc
```

in the service you will be able to see the service running with Nodeport 

check the ip of your service /instance using . If using minikube you can check it using 
```
minikube ip
```

to access the application 

```
curl -L <http://ip :30007/demo>
```

Or u can directly access your application in the browser itself

```
192.168.49.2:30007/demo
```

### to make the service accessible to the outside world

```
kubectl edit svc <svc-name>
```

```
kubectl edit svc py-django-app-sv
```

change the type of Nodeport to LoadBalancer . It will only work on cloud system and will be controlled by cloud controller manager - (CCM)

After changing it to Loadbalancer u will get the external IP on get svc page

```
kubectl get svc 
```

for exposing you minikube projects to external world you can use MetalLB (still in beta) .

### basic functionality of kubernetes service

- Service discovey
- Loadbalancing
- Exposing application to external world


### installing kubeshark to demonstrate the loadbalancing feature of kubernetes

for linux
```
sh <(curl -Ls https://kubeshark.co/install)
```

to run kubeshark

```
kubeshark tap
```

kubeshark runs on the localhost on port 8899
http://localhost:8899

use the curl command on the terminal for the application ip

```
curl 192.168.49.2:30007/demo
curl 192.168.49.2:30007/demo
curl 192.168.49.2:30007/demo
curl 192.168.49.2:30007/demo
curl 192.168.49.2:30007/demo
curl 192.168.49.2:30007/demo
curl 192.168.49.2:30007/demo

```
```
kubectl get pods -o  wide
```
here we will be able to see IPs for both the pod and in kubeshark we will be able to see the requests going to both the IPs in a round robin scheduling manner .

![image](https://github.com/yo-404/Kubernetes/assets/100558220/eb9891f6-47ea-41c0-8ece-aacb7a518f25)



