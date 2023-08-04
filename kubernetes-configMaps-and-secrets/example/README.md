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

# Secret

### how to create a secret from kubectl

`kubectl create secret generic test-secret --from-literal=db-port="3306" `

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

### security of secret in k8s

- For encrypting the secret we can also use Hashicorp vault 
- enforcing strict RBAC policies
- Improve etcd management policies
- Avoid sharing Secret manifests
- Restrict Secret access to specific containers