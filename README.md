# k8s-assignment
The objective of this assignment is to deploy an NGINX application using Kubernetes resources, ensuring secure access through basic authentication and exposing it to external access via Ingress.

## Components:
### ConfigMap:

Defines NGINX configuration (nginx.conf) with basic settings and authentication setup.
### Secret:

Stores sensitive information such as username and password for NGINX basic authentication.
### Deployment:

Runs NGINX container configured with an initContainer to initialize basic authentication using data from nginx-config ConfigMap and example-secret Secret.
### Service:

Exposes the NGINX deployment internally within the Kubernetes cluster.
### Ingress:

Routes external HTTP traffic to the NGINX service, enabling access from outside the Kubernetes cluster.

## Steps

### Pre Requisites

Install KinD and kubectl CLIs
refer:
- [KinD binary](https://kind.sigs.k8s.io/docs/user/quick-start#installing-from-release-binaries)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux)

### Create KinD cluster

Use the cluster config to create the KinD cluster
```
kind create cluster --config=./cluster-config
```

Once the cluster has been created check if the nodes are up
```
kubectl get nodes
```

### Create ConfigMap

Create the configmap (configmap file `cm.yaml` doesn't have any changes), we are setting the basic auth for nginx

### Create Secret

To create the secret put the base64 values for a username and password in the `secret.yaml`, then create the secret.

### Create Deployment

To create the deployment update the placeholders (`<secret-key>`, `<nginx-config-volume-name>`) with the correct vaules in the file `deploy.yaml`. Then create the deployment. Once the deployment has been created check if the pod is up and running.

Note: Do not Change anything in the command field.

### Create Service

To create the service update the `selector` and `<container-port>` fields in the `svc.yaml`. Then create the service.

### Create Ingress

First Install the nginx controller

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

Check in the `kube-system` namespace to make sure the controller pods are up.

To create the ingress update the `<service-name> and <service-port> with the appropriate values, so that the ingress can route traffic to the nginx deployments' service. Then create the ingress.

### Verify

When the ingress resource is created, make an entry in the `/etc/hosts/` file
```
# example, if the IP address for ingress is localhost then the entry will be
127.0.0.1 example.com
```

Open the browser then enter `http://example.com`, it should prompt for the username and password. Provide the username and password (actual values) and see if the webpage is accessible. 
```
