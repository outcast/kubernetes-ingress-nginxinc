# Security

The security of the Ingress Controller is paramount to the success of our customers. When the NGINX Ingress Controller is deployed your environment and you take responsibility we do recommend the review of practices to secure a deployment of the NGINX Ingress Controller.
We strongly recommend every operator read and understand the following items:

## Kubernetes

We recommend reading and applying practices from the Kubernetes [guide to securing a cluster](https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/).

The NGINX Ingress Controller is deployed within a Kubernetes environment and interacts with the Kubernetes API. Following Kubernetes best practices we recommend:

### Role based access control (RBAC) and the service account

Kubernetes uses [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) to control the resources and operations available to different accounts.
The NGINX Ingress Controller requires a service account which should be limited to its lowest necessary privilege.
We strongly recommend using the [RBAC configuration](https://github.com/nginxinc/kubernetes-ingress/blob/master/deployments/rbac/rbac.yaml) provided in our standard deployment configuration.
This already provides the least amount of privilege required for the NGINX Ingress Controller.

We strongly recommend inspecting the RBAC configuration (for [manifests installation](https://github.com/nginxinc/kubernetes-ingress/blob/master/deployments/rbac/rbac.yaml)
or for [helm](https://github.com/nginxinc/kubernetes-ingress/blob/master/deployments/helm-chart/templates/rbac.yaml))
to understand what access the NGINX Ingress Controller service account has and to which resources.
For example, by default the service account has access to all Secret resources in the cluster so they can be easily used within your deployments, but your security policies might need this to be restricted to specific secrets.

### Certificates and Privacy Keys

Secrets are required by the Ingress Controller for some configurations.
[Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) are stored by Kubernetes in an unencrypted way by default.
We strongly recommend configuring Kubernetes to store your secrets encrypted while at rest (written to disk or source control).
Kubernetes has [documentation](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/) describing how to configure this.

## NGINX Ingress Controller

### Securing Prometheus

If Prometheus metrics are [enabled](/nginx-ingress-controller/configuration/global-configuration/command-line-arguments/#cmdoption-enable-prometheus-metrics),
we recommend [configuring the Prometheus endpoint using HTTPS](/nginx-ingress-controller/configuration/global-configuration/command-line-arguments/#cmdoption-prometheus-tls-secret).

### Snippets

Snippets are a powerful feature necessary to implement some scenarios with the NGINX Ingress Controller.
[Snippets](/nginx-ingress-controller/configuration/ingress-resources/advanced-configuration-with-snippets/)

Due to the power of the the snippets capability, we will be disabling it by default in a future release.
We strongly encourage that you understand the configurations you provide through the snippets capability and we encourage thorough internal code reviews and testing to ensure the resulting configuration is as secure as you intended.
