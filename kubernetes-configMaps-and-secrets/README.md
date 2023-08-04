# ConfigMaps and secrets in K8s

### What is a configmap in kubernetes

 ConfigMap is an API resource used to store non-sensitive configuration data in key-value pairs. It allows you to decouple the configuration of your application from the container image, making it easier to manage and update configurations without rebuilding the image.

ConfigMaps are commonly used for providing configuration data to applications running in containers. They are especially useful for applications that need to be deployed in multiple environments (e.g., development, staging, production) with different configurations.

- Key-Value Pairs: ConfigMaps store configuration data as key-value pairs. Each key represents a specific configuration property, and its corresponding value holds the configuration data.

- Sources of Configuration: ConfigMaps can be created from various sources, such as literal values in YAML manifests, files, or even from environment variables.

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

## steps for example are provided in `/example/python-web-app/README.md`

### security of secret in k8s

- For encrypting the secret we can also use Hashicorp vault 
- enforcing strict RBAC policies
- Improve etcd management policies
- Avoid sharing Secret manifests
- Restrict Secret access to specific containers