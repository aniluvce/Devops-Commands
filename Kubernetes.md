**Deployment YAML Files:**

Deployment YAML files in Kubernetes are used to define and manage a set of identical pods. They ensure that a specified number of pod replicas are running and handle rolling updates and rollbacks. Deployments provide declarative updates for pods and are widely used for managing stateless applications.

Here's an example of a Deployment YAML file:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:latest
        ports:
        - containerPort: 80
```

In this example:
- `apiVersion` specifies the Kubernetes API version being used.
- `kind` defines the type of Kubernetes object being created (in this case, a Deployment).
- `metadata` contains information about the Deployment, such as its name.
- `spec` specifies the desired state of the Deployment.
- `replicas` defines the desired number of pod replicas.
- `selector` is used to match the pods controlled by the Deployment.
- `template` defines the pod template used to create the replicas.
- `containers` specifies the list of containers running within the pod template.

**Service YAML Files:**

Service YAML files in Kubernetes are used to define a stable endpoint for accessing a group of pods. Services enable load balancing and service discovery within the cluster. They provide an abstraction layer that allows pods to be replaced or scaled without affecting the application's accessibility.

Here's an example of a Service YAML file:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: NodePort
```

In this example:
- `apiVersion` specifies the Kubernetes API version being used.
- `kind` defines the type of Kubernetes object being created (in this case, a Service).
- `metadata` contains information about the Service, such as its name.
- `spec` specifies the desired state of the Service.
- `selector` is used to match the pods that the Service will target.
- `ports` defines the list of ports to be exposed by the Service.
- `protocol` specifies the protocol to be used (in this case, TCP).
- `port` specifies the port number for accessing the Service.
- `targetPort` specifies the port number on the pods to which traffic should be forwarded.
- `type` defines the type of Service. `NodePort` exposes a service externally to the cluster by means of the target nodes IP address and the NodePort.


## --- KUBERNETES Install Steps ---
```markdown


## Step1:

### On Master and slave


apt-get update -y
apt-get install docker.io -y
service docker restart
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" >/etc/apt/sources.list.d/kubernetes.list
apt-get update
apt install kubeadm=1.20.0-00 kubectl=1.20.0-00 kubelet=1.20.0-00 -y
```

## Step2:

### On Master node:

```bash
kubeadm init --pod-network-cidr=192.168.0.0/16
```

## Step3:

### On Master node:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Step4:

### On Master node:

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/calico.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.49.0/deploy/static/provider/baremetal/deploy.yaml
```


### Kubectl is a command-line tool used to interact with Kubernetes clusters. It allows you to deploy and manage applications, inspect and modify cluster resources, and perform various administrative tasks. Here are some basic kubectl commands:

1. **kubectl get**: Retrieve information about resources in the cluster.
   - `kubectl get pods`: List all pods in the cluster.
   - `kubectl get deployments`: List all deployments in the cluster.
   - `kubectl get services`: List all services in the cluster.
   - `kubectl get nodes`: List all nodes in the cluster.

2. **kubectl describe**: Get detailed information about a specific resource.
   - `kubectl describe pod <pod-name>`: Show detailed information about a specific pod.
   - `kubectl describe deployment <deployment-name>`: Show detailed information about a specific deployment.
   - `kubectl describe service <service-name>`: Show detailed information about a specific service.

3. **kubectl create**: Create a resource from a file or inline definition.
   - `kubectl create -f <file.yaml>`: Create a resource defined in a YAML file.
   - `kubectl create deployment <deployment-name> --image=<image-name>`: Create a deployment from a specified container image.

4. **kubectl delete**: Delete a resource.
   - `kubectl delete pod <pod-name>`: Delete a specific pod.
   - `kubectl delete deployment <deployment-name>`: Delete a specific deployment.
   - `kubectl delete service <service-name>`: Delete a specific service.

5. **kubectl apply**: Apply changes to a resource.
   - `kubectl apply -f <file.yaml>`: Apply changes to a resource defined in a YAML file.
   - `kubectl apply -f <dir>`: Apply changes to all resources defined in a directory.

6. **kubectl logs**: View the logs of a pod.
   - `kubectl logs <pod-name>`: Print the logs of a specific pod.

7. **kubectl exec**: Execute a command inside a container of a pod.
   - `kubectl exec -it <pod-name> -- <command>`: Run an interactive shell in a specific pod.

8. **kubectl port-forward**: Forward local ports to a pod.
   - `kubectl port-forward <pod-name> <local-port>:<pod-port>`: Forward a local port to a specific pod.

These are just a few examples of the most commonly used kubectl commands. There are many more commands available, and you can explore additional options and parameters in the Kubernetes documentation or by using `kubectl --help`.
