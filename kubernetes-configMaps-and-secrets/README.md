# ConfigMaps and secrets in K8s

### What is a configmap in kubernetes

 ConfigMap is an API resource used to store non-sensitive configuration data in key-value pairs. It allows you to decouple the configuration of your application from the container image, making it easier to manage and update configurations without rebuilding the image.

ConfigMaps are commonly used for providing configuration data to applications running in containers. They are especially useful for applications that need to be deployed in multiple environments (e.g., development, staging, production) with different configurations.

- Key-Value Pairs: ConfigMaps store configuration data as key-value pairs. Each key represents a specific configuration property, and its corresponding value holds the configuration data.

- Sources of Configuration: ConfigMaps can be created from various sources, such as literal values in YAML manifests, files, or even from environment variables.

![image](https://github.com/yo-404/Kubernetes/assets/100558220/7c600827-ffbb-47dd-91f0-96ed11fdf754)


### Creating ConfigMaps
 
ConfigMaps can be created using the `kubectl create configmap `command or by providing a YAML manifest that describes the ConfigMap's data.

Example YAML for creating a ConfigMap:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: example-configmap
data:
  key1: value1
  key2: value2
  key3: value3

```

### what is secret in kubernetes

Secrets are used to store sensitive configuration data, such as passwords, API tokens, certificates, SSH keys, etc. They are intended for confidential information that should not be exposed in plain text.
Secret data is stored in an encrypted format inside etcd, the Kubernetes datastore. This encryption ensures that sensitive data is not readily accessible.

Secrets can be created using kubectl create secret or by providing a YAML manifest. Data can be set using key-value pairs, or in some cases, by directly referencing a file containing the secret data.

### Creating a Secret from a file:

If you have sensitive data stored in a file, you can create a Secret using the contents of that file:

```
kubectl create secret generic <secret-name> --from-file=<path-to-file>
```


Replace <secret-name> with the desired name for your Secret, and <path-to-file> with the path to the file containing the sensitive data.

###  Using YAML manifest:

 example for YAML manifest for creating a Secret:

```
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: <base64-encoded-username>
  password: <base64-encoded-password>

```

```
kubectl apply -f my-secret.yaml
```

### to check configmap present in k8s cluster

```
kubectl get cm
```

### to delete configmap present in k8s cluster

```
kubectl delete cm <cm-name>
```

## steps for example are provided in `/example/README.md`

### security of secret in k8s

- For encrypting the secret we can also use Hashicorp vault 
- enforcing strict RBAC policies
- Improve etcd management policies
- Avoid sharing Secret manifests
- Restrict Secret access to specific containers

=======================================================================================================
# Example  

```
vi cm.yaml
```
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: test-cm
data:
  db-port: "3306"
  username: yogesh932
  
```
`kubectl apply -f cm.yaml`


### using the same python deployments

deploying the py application 

`vi deployment.yaml`

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
        env:
         - name: DB-PORT
           valueFrom:
            configMapKeyRef:
             name: test-cm
             key: db-port   
        ports:
        - containerPort: 8000
```

```
kubectl apply -f deplyment.yaml
```
ssh into one of the pods created for the app

`env | grep DB`

since the key value pair is stored in env now and hence the user can use it inside the application 

`os.env("DB-PORT")`

![image](https://github.com/yo-404/Kubernetes/assets/100558220/a19a82c0-d2ff-4f9e-9838-c378e6edeee7)


any changes inside the cm.yaml will not be reflected inside the env . For changing values user should use volume mounts as env values inside containers dont change . 

### using volume mounts for configMaps

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
        volumeMounts:
         - name: db-connection
           mountPath: /opt
        ports:
        - containerPort: 8000
      volumes:
        - name: db-connection
          configMap:
            name: test-cm  

```
`kubectl apply -f deployment.yaml`

```
kubectl exec -it sample-py-app-7dfb77486b-vvkgr /bin/bash
env | grep db
ls /opt
cat /opt/db-port | more
cat /opt/username | more
```

![image](https://github.com/yo-404/Kubernetes/assets/100558220/78bcaaf9-7d61-46bd-8ae4-5eda888aafb7)


### changing cm.yaml file to see if changes are reflected in volume mount

`vi cm.yaml`

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: test-cm
data:
  db-port: "3308"
  username: yogesh932
```

`kubectl apply -f cm.yaml`

ssh into the pod and check the volume mount for the port . It should be changed within few seconds 

![image](https://github.com/yo-404/Kubernetes/assets/100558220/e9b19f5f-329e-4728-8a08-909324e506ac)


# Secret

### how to create a secret from kubectl

`kubectl create secret generic test-secret --from-literal=db-port="3306" `

![image](https://github.com/yo-404/Kubernetes/assets/100558220/4764ff35-0be5-4eef-877b-c4d31041348b)


### to create secret from yaml

```
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: <base64-encoded-username>
  password: <base64-encoded-password>

```

to apply 

`kubectl apply -f my-secret.yaml`

`kubectl describe secret test-secret`

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
        volumeMounts:
         - name: db-connection
           mountPath: /opt
        ports:
        - containerPort: 8000
      volumes:
        - name: db-connection
          secret:
            name: test-secret  
```

`kubectl apply -f deployment.yaml`

![image](https://github.com/yo-404/Kubernetes/assets/100558220/c09e59a9-0e9d-4782-842d-5c4f9dc13ed6)


### security of secret in k8s

- For encrypting the secret we can also use Hashicorp vault 
- enforcing strict RBAC policies
- Improve etcd management policies
- Avoid sharing Secret manifests
- Restrict Secret access to specific containers
