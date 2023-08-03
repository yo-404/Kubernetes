# Introduction to k8s RBAC

RBAC stands for role based access control

Role-Based Access Control (RBAC) is used in Kubernetes to manage and enforce fine-grained access controls for various resources within the cluster. RBAC allows you to define who (users and groups) has what level of access (roles and permissions) to specific Kubernetes objects (e.g., pods, services, namespaces, etc.).

 RBAC provides several key benefits in Kubernetes:

    *Security*: RBAC enhances the security of your Kubernetes cluster by restricting access to sensitive resources. It ensures that only authorized users or processes can perform specific actions, reducing the risk of unauthorized access and potential security breaches.

    *Granular Control*: With RBAC, you can define detailed access controls at the individual resource level. This allows you to grant different levels of access to different users and groups based on their roles and responsibilities.

    *Least Privilege Principle*: RBAC follows the principle of least privilege, which means that users are given the minimum necessary permissions to perform their tasks. This reduces the risk of accidental or intentional misuse of privileges.

    *Separation of Duties*: RBAC enables you to separate administrative responsibilities within the cluster. You can create roles that grant only specific actions to certain users, avoiding giving full cluster-wide access to everyone.

    *Scalability*: As your Kubernetes cluster grows and more users are added, RBAC provides a scalable approach to managing access controls without sacrificing security.

    *Compliance*: Many organizations need to adhere to various compliance standards, and RBAC helps in meeting these requirements by ensuring controlled access to resources.

### How RBAC is managed in kubenetes

- Service accounts/users
- roles/cluster roles
- Role binding / cluster role binding

Kubernetes does not manage the users but it offloads the work to identity providers for the same

In kubernetes the API server works as the OAUTH server which can offload the user management to any external identity provider.

there are several ways to enable external OAuth integration in Kubernetes using third-party tools and authentication mechanisms. Here are some popular options:

    OAuth2 Proxy: OAuth2 Proxy is a widely used open-source tool that can be deployed alongside Kubernetes to enable OAuth authentication. It acts as a reverse proxy, intercepting requests and handling the OAuth2 flow with an external identity provider (e.g., Google, GitHub, Okta). Once authenticated, the proxy passes the requests to Kubernetes API server with appropriate headers, granting access based on the authenticated user's role.

    Keycloak: Keycloak is an open-source identity and access management solution that supports OAuth and other authentication protocols. It can be integrated with Kubernetes to provide external OAuth authentication and RBAC integration, enabling single sign-on and centralized user management.

    Dex: Dex is an open-source identity provider that supports OAuth 2.0 and OpenID Connect. It can be used in conjunction with Kubernetes to enable external OAuth authentication. Dex allows you to integrate with various identity providers, making it flexible for different environments.

    Auth0: Auth0 is a cloud-based identity platform that provides OAuth and other authentication mechanisms. It can be integrated with Kubernetes clusters to enable external OAuth authentication and single sign-on capabilities.

    Azure Active Directory (Azure AD): If you are using Azure Kubernetes Service (AKS), you can integrate it with Azure AD, which provides OAuth support. This allows you to use Azure AD identities for authentication and RBAC in AKS.


### service account

Service Accounts are used to provide an identity for processes running in pods. These service accounts can be used by applications or processes within the pods to authenticate themselves with the Kubernetes API server and access various resources based on the permissions granted to the service account.

user can create Service Account using a YAML manifest file. Here's an example of a basic Service Account definition:

service-account.yaml
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
  namespace: my-namespace  # Optional: Define the namespace in which the Service Account will be created

```
to apply settings

```
kubectl apply -f service-account.yaml

```
using kubectl

```
kubectl create serviceaccount my-service-account

```
This will create a Service Account named my-service-account in the current namespace.

Service Account Auto-Creation (Default):

By default, each namespace in Kubernetes is automatically associated with a "default" Service Account. If you don't specify a Service Account for a pod, it will use the "default" Service Account in that namespace.

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx

```

The pod will automatically use the "default" Service Account of the namespace where it is created.

Once you have created a Service Account, you can use it in a pod's specification by specifying the serviceAccountName field:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  serviceAccountName: my-service-account
  containers:
  - name: my-container
    image: nginx

```

if the roles are created within a specific namespace it will be called as role whereas if the role is created within the scope of the cluster it will be known as cluster role.

### role binding vs cluster role binding

 both Role Binding and Cluster Role Binding are used to associate permissions (Roles or ClusterRoles) with subjects (Users, Groups, or ServiceAccounts) to grant access to various resources within the cluster.

 - Role binding : Role Binding is used to bind a specific Role to a subject within a particular namespace. It allows you to grant permissions for resources within that namespace only .Role Binding is typically used when you want to provide fine-grained access control to resources within a specific namespace.

example
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: my-role-binding
  namespace: my-namespace
subjects:
- kind: User
  name: alice
roleRef:
  kind: Role
  name: my-role
  apiGroup: rbac.authorization.k8s.io

```

 - Cluster Role Binding : Cluster Role Binding is used to bind a ClusterRole to a subject across the entire cluster. It allows you to grant permissions for resources across all namespaces in the cluster .Cluster Role Binding is typically used when you want to provide broad permissions that span multiple namespaces or resources within the whole cluster .It is useful for granting access to cluster-level resources like nodes, persistent volumes, or namespaces. 

 example

 ```
 apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: my-cluster-role-binding
subjects:
- kind: Group
  name: developers
roleRef:
  kind: ClusterRole
  name: my-cluster-role
  apiGroup: rbac.authorization.k8s.io

 ```