# Security

We consider the security of the Ingress Controller paramount to the success of our users and use the following to 
ensure the security of the Ingress Controller:
* [Secure Development Life Cycle](https://www.microsoft.com/en-us/securityengineering/sdl/)
* [FOSSA](https://fossa.com) scanning

However, the Ingress Controller is deployed by a User in their environment, and as such, the User takes responsibility 
for securing a *deployment* of the Ingress Controller. 
We highly recommend every User to read and understand the following security concerns.

## Kubernetes
We recommend the Kubernetes [guide to securing a cluster](https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/).
In addition, the following relating more specifically to Ingress Controller.

### RBAC and Service Account
The Ingress Controller is deployed within a Kubernetes environment, this environment must be secured. 
Kubernetes uses [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) to control which types of users 
can access which resources and what operations they can perform. 
The Ingress Controller requires a service account which is configured using RBAC. 
We strongly recommend using the [RBAC configuration](https://github.com/nginxinc/kubernetes-ingress/blob/master/deployments/rbac/rbac.yaml) provided in our standard deployment configuration. 
It is configured with the least amount of privilege required for the Ingress Controller to work.

### Certificates and Privacy Keys
Secrets are required by the Ingress Controller for some configurations. 
[Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) are stored by Kubernetes unencrypted by default. 
We highly recommend configuring Kubernetes to store these Secrets encrypted at rest. 
Kubernetes has [documentation](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/) on how to configure this.

## Ingress Controller

### Recommended Secure Defaults
We recommend the following for the most secure configuration:
 * If Prometheus metrics are [enabled](https://docs.nginx.com/nginx-ingress-controller/configuration/global-configuration/command-line-arguments/#cmdoption-enable-prometheus-metrics), 
   we recommend [configuring HTTPS](https://docs.nginx.com/nginx-ingress-controller/configuration/global-configuration/command-line-arguments/#cmdoption-prometheus-tls-secret) for Prometheus.

### Snippets
[Snippets](https://docs.nginx.com/nginx-ingress-controller/configuration/ingress-resources/advanced-configuration-with-snippets/) 
are a powerful feature which enable the injection of NGINX config. 
This can be useful for a proof of concept or debugging. 
Snippets are disabled by default as they can be used to inject malicious config. 
We recommend they are not used in production.
