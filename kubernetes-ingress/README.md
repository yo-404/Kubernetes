# Kubernetes Ingress

### why kubernetes ingress

kubernetes ingress provides the additional functionality that kubernetes initially didnt provide . Initially kubernetes used services to provide the enterprise level features like service discover , load balancing and exposing application to external world . Though the loadbalancing feature has some serious issues as it only provided the Round robin approach for balancing applications. Also , for each service created in kubernetes ,to expose it to the external world there was a requirement for static IP for each listed services .Since ,for an application with 100s of services required 100s of static IPs . Therefore it was getting expensive in terms of cost as well . 

In November 18 2015  kubernetes introduced Ingress which was a significant milestone for kubernetes. With the introduction of Ingress, Kubernetes users gained a unified API to define rules for routing external traffic to services based on hostnames and paths. This made it easier to manage and configure external access to applications running on Kubernetes, paving the way for better load balancing, SSL termination, and other features provided by Ingress controllers. 

Ingress can be used for applications like Nginx , F5load balancer , HAProxy etc . Ingress can help kubernetes provide multiple type of functionalitites in load balancing such as -

- ratio based load balancing
- sticky sessions
- path based loadBalancing
- Domain based LoadBalancing
- Whitelisting
- Blacklisting
- TLS
- Host based 

To implement Ingress , we need to add the ingress controller from the Loadbalancer service providers of our choice . In order to use it ,user must add an Ingress controller into the application which can understand the ingress resource yaml files . The user can then add his requirement as per need into the YAML file .


## kubernetes ingress resource YAML format

ingress.yml
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-example
spec:
  rules:
  - host: "foo.bar.com"
    http:
      paths:
      - pathType: Prefix
        path: "/bar"
        backend:
          service:
            name: my-service-py-app
            port:
              number: 80
  
```
```
kubectl apply -f ingress.yml
```
```
kubectl get ingress
```

for k8s to understand the ingress resource yaml file it needs to have a ingress controller . Here we are going to install nginx ingress controller 

steps for installing nginx controller can be found on their offical website documentation section

https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests/


if using minikube , you can use 
```
minikube addons enable ingress
```
check if the controllers are enabled and are running using

```
kubectl get pods -A | grep nginx
```

adding the foo.bar.com domain in hosts list 
```
sudo vi /etc/hosts
```        
```
<ip of application> foo.bar.com
```
try pinging the to foo.bar.com and it should work 

also curl command should also be working fine now and the application is now accessible

```
curl -L http://foo.bar.com/bar -v
```




