# Custom resource in K8s

### Why custom Resource 

Custom Resource (CR) is a powerful feature that allows you to define and extend the Kubernetes API with your custom objects. These custom objects are essentially a way to represent and manage application-specific resources that are not part of the core Kubernetes API.

### resources to extend the API

- CRD : custom resource definition
- CR : custom Resource
- Custom controller

A crd is a yaml file which is used to introduce the new type of API to kubernetes and it contains all the fields that a user can submit inside the custom resource

Custom resource is a basic YAML file which enlists all the requirements that are needed to be fulfilled / features that are to be extended . It is later validated to the CRD . Once it is validated the custom controller is responsible for deployments of custom resources 

### Why custom resource definitions are used

- Applications: Kubernetes provides a set of core resources like pods, services, deployments, etc., which are essential for managing containerized applications. However, many applications have specific requirements and configurations that cannot be fully represented by the core resources. CRDs allow developers to define their own resources that are tailored to their application's needs.

- Abstraction and Simplification: Custom Resources provide a higher level of abstraction, making it easier for developers and operators to interact with complex systems. They hide implementation details and present a more user-friendly interface for managing specific functionalities or applications.

- Declarative Configuration: Kubernetes, as a declarative platform, allows users to define the desired state of their application, and the Kubernetes controller will strive to make the current state match the desired state. Custom Resources enable you to declare the desired state of your custom objects, and Kubernetes controllers take care of the actual implementation.

- Integration with Kubernetes Ecosystem: By creating Custom Resources, you can integrate your application more seamlessly with the Kubernetes ecosystem. This integration includes using standard Kubernetes tooling, like kubectl, to manage your custom objects, as well as leveraging other Kubernetes features like RBAC (Role-Based Access Control) and admission controllers.

- Operator Pattern Implementation: Custom Resources are often used in conjunction with Operators. Operators are custom controllers that act upon Custom Resources to automate the management of applications and their components. This allows you to implement application-specific logic and automation for deployment, scaling, backup, recovery, etc.

- Third-Party Integrations: Custom Resources enable third-party developers to create extensions and integrations with Kubernetes. This extensibility encourages the development of a rich ecosystem of tools, plugins, and frameworks around Kubernetes.

- Consistency and Reusability: Custom Resources promote consistency in managing applications and configurations. By defining a custom resource for a specific application or component, you can reuse it across different environments and clusters.