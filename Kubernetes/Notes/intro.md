# 1. Introduction to Kubernetes

Kubernetes, also known as K8s, is a powerful open-source system for automating the deployment, scaling, and management of containerized applications.

**Key Concepts**

Cluster: A set of worker machines, called nodes, that run containerized applications.

Node: A single machine in a Kubernetes cluster.

Pod: The smallest deployable unit, which can contain one or more containers.

Service: A stable network endpoint to expose a set of pods.

Namespace: A way to divide cluster resources between multiple users.

Kubelet: An agent running on each node that ensures containers are running in a Pod.

Kubectl: Command-line tool to interact with Kubernetes clusters.

Deployment: A higher-level abstraction that manages a replicated application, ensuring that a specified number of replicas are running.

Service: An abstraction that defines a logical set of pods and a policy by which to access them.

ConfigMap and Secret: Mechanisms to inject configuration data into your applications.

# 2. Basic Kubernetes Operations

**Setting Up a Kubernetes Cluster**

Minikube: Set up a single-node Kubernetes cluster for testing.
````bash
minikube start
````
**Working with kubectl**

Get Cluster Information:
````bash
kubectl cluster-info
````
Get All Nodes in the Cluster:
````bash
kubectl get nodes
````

Managing Pods

Creating a Pod
````bash
kubectl run mypod --image=nginx
````
Listing all Pods
````bash
kubectl get pods
````
Describing a Pod
````bash
kubectl describe pod mypod
````
Deleting a Pod
````bash
kubectl delete pod mypod
````

**Using Namespaces**

Listing All Namespaces:
````bash
kubectl get namespaces
````
Create a Namespace:
````bash
kubectl create namespace mynamespace
````
Deleting a Namespace:
````bash
kubectl delete namespace mynamespace
````

## 3. Deployments and Scaling

List Deployments
````bash
kubectl get deployments
kubectl get deployments -n my-namespace
````
Lists all deployments in the default namespace or a specified namespace.

Create Deployment
````bash
kubectl create deployment my-deployment --image=nginx
````
Creates a new deployment with the specified container image.

View Deployment Status:
````bash
kubectl get deployments
````
Update a Deployment:
````bash
kubectl set image deployment/myapp nginx=nginx:1.16
````
Rollout Status
````bash
kubectl rollout status deployment/my-deployment
````

Rollback a Deployment:
````bash
kubectl rollout undo deployment/myapp
````
Delete Deployment
````bash
kubectl delete deployment my-deployment
````
**Scaling Applications**

Scale a Deployment:
````bash
kubectl scale deployment myapp --replicas=3
````
Auto-scaling with Horizontal Pod Autoscaler (HPA):
````bash
kubectl autoscale deployment myapp --min=1 --max=5 --cpu-percent=80
````
**Service Management**

List Services
````bash
kubectl get services
kubectl get services -n my-namespace
````
Lists all services in the default namespace or a specified namespace.

Create Service
````bash
kubectl expose deployment my-deployment --type=LoadBalancer --name=my-service
````
Creates a new service to expose a deployment.

Deleting a Service
````bash
kubectl delete service my-service
````
Deletes a specified service.


## 🚀 Intermediate Commands

**ConfigMap and Secret Management**

List ConfigMaps
````bash 
kubectl get configmaps
kubectl get configmaps -n my-namespace
````
Lists all ConfigMaps in the default namespace or a specified namespace.

Create ConfigMap
````bash
kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
````
Creates a new ConfigMap from literal values.

List Secrets
````bash
kubectl get secrets
kubectl get secrets -n my-namespace
````
Lists all secrets in the default namespace or a specified namespace.

Create Secret
````bash
kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password=secret
````
Creates a new secret from literal values.

**Monitoring and Logging**

View Pod Logs
````bash
kubectl logs my-pod
````
Displays the logs of a specified pod.

View Previous Pod Logs

````bash
kubectl logs my-pod --previous
````
Displays the logs of a previous instance of a specified pod.

Get Events
````bash
kubectl get events
kubectl get events -n my-namespace
````
Lists all events in the default namespace or a specified namespace.

**Resource Management**

Describe Node
````bash
kubectl describe node my-node
````
Displays detailed information about a specified node.

Label Nodes
````bash
kubectl label nodes my-node disktype=ssd
````
Adds a label to a node.

Annotate Resources
````bash
kubectl annotate pod my-pod description="my example pod"
````
Adds an annotation to a resource.

**Network Policies**

Create Network Policy
````bash
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-nginx
  namespace: my-namespace
spec:
  podSelector:
    matchLabels:
      app: nginx
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
````
Apply the network policy:

````bash
kubectl apply -f network-policy.yaml
````

## 🧠 Advanced Commands

**Advanced Pod Management**

Debug Pod
````bash
kubectl debug my-pod --image=busybox --target=my-container
````
Debugs a running pod by creating a new debugging container in the pod.

Port Forwarding
````bash
kubectl port-forward my-pod 8080:80
````
Forwards a local port to a port on a pod.

**Advanced Node Management**

Cordon Node
````bash
kubectl cordon my-node
````
Marks a node as unschedulable.

Drain Node
````bash
kubectl drain my-node --ignore-daemonsets
````
Safely evicts all pods from a node before maintenance.

Uncordon Node
````bash
kubectl uncordon my-node
````
Marks a node as schedulable.

**Resource Quotas and Limits**

Create Resource Quota
````bash
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
  namespace: my-namespace
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 16Gi
    limits.cpu: "10"
    limits.memory: 32Gi
````
Apply the resource quota:
````bash
kubectl apply -f resource-quota.yaml
````

**Custom Resources and CRDs**

Create Custom Resource Definition
````bash
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: crontabs.stable.example.com
spec:
  group: stable.example.com
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                cronSpec:
                  type: string
                image:
                  type: string
                replicas:
                  type: integer
  scope: Namespaced
  names:
    plural: crontabs
    singular: crontab
    kind: CronTab
    shortNames:
    - ct
````
Apply the CRD:
````bash
kubectl apply -f crd.yaml
````
**Operators**

Introduction to Operators: Operators are Kubernetes applications designed to manage complex stateful applications by extending the Kubernetes API.

Creating an Operator:

Use the Operator SDK to scaffold and build an operator.
````bash
operator-sdk init --domain=example.com --repo=github.com/example/memcached-operator
operator-sdk create api --group=cache --version=v1 --kind=Memcached --resource --controller
````
**Service Mesh with Istio**

Install Istio:
````bash
istioctl install --set profile=demo
````
Deploy an Application with Istio:

Annotate namespace to enable Istio sidecar injection.
````bash
kubectl label namespace mynamespace istio-injection=enabled
````
Deploy application in the annotated namespace.

**Traffic Management with Istio:**

Create VirtualService and DestinationRule to manage traffic routing.
````bash
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: myapp
spec:
  hosts:
  - myapp.example.com
  http:
  - route:
    - destination:
        host: myapp
        subset: v1
````

Create Custom Resource
````bash
apiVersion: stable.example.com/v1
kind: CronTab
metadata:
  name: my-new-cron-object
  namespace: my-namespace
spec:
  cronSpec: "* * * * */5"
  image: my-cron-image
  replicas: 3
````
Apply the custom resource:
````bash
kubectl apply -f custom-resource.yaml
````

## Helm for Kubernetes Package Management

Installing Helm

Follow the installation instructions for Helm from the [official Helm website](https://helm.sh/docs/intro/install/).

Add Helm Repository
````bash
helm repo add stable https://charts.helm.sh/stable
````
Install Helm Chart
````bash
helm install my-release stable/nginx
````
List Helm Releases
````bash
helm list
````
Upgrade Helm Release
````bash
helm upgrade my-release stable/nginx
````
Uninstall Helm Release
````bash
helm uninstall my-release
````
## 📊 Best Practices

Use Namespaces

for Isolation

-Organize resources by namespaces for better management and access control.

Label Resources

-Use labels to organize and select resources efficiently.

Monitor and Log

-Continuously monitor your cluster and collect logs for troubleshooting and performance analysis.

Automate with Scripts

-Use scripts to automate repetitive tasks and ensure consistency.

Secure Your Cluster

-Implement RBAC, network policies, and secure access controls to protect your cluster.

## Monitoring and Logging(part 2)

Prometheus and Grafana:

Install Prometheus:
````bash
kubectl apply -f https://github.com/prometheus-operator/prometheus-operator/blob/main/bundle.yaml
````
Install Grafana:
````bash
kubectl apply -f https://raw.githubusercontent.com/grafana/grafana/main/deploy/kubernetes/grafana-deployment.yaml
````
View Metrics in Grafana: Access the Grafana dashboard and configure data sources to use Prometheus.

Logging with ELK Stack:

Deploy ELK Stack: Use Helm or custom YAML manifests to deploy Elasticsearch, Logstash, and Kibana.
````bash
helm install elk-stack stable/elastic-stack
````
Configure Fluentd for Log Collection:

Deploy Fluentd as a DaemonSet to collect logs from all nodes and send them to Elasticsearch.

**High Availability and Disaster Recovery**

Kubernetes High Availability (HA):

HA Master Nodes: Set up multiple master nodes to ensure availability.
HA etcd Cluster: Use an HA etcd cluster to store Kubernetes state with redundancy.
Disaster Recovery:

Backup and Restore etcd:

Use etcdctl to take snapshots of the etcd cluster.

````bash
etcdctl snapshot save /path/to/backup
````
Restore from the snapshot when needed.

Federation

Multi-Cluster Federation:

Set Up Federation: Use Kubernetes Federation v2 to manage multiple clusters from a single control plane.
````bash
kubefedctl join mycluster --cluster-context=mycluster-context --host-cluster-context=host-cluster-context
````
Deploy Federated Resources: Deploy resources that span across multiple clusters using the Federation API.

## References

**Official Documentation**

[Kubernetes Official Documentation](https://kubernetes.io/docs/home/)

**Community Resources**

[Kubernetes Slack](https://communityinviter.com/apps/kubernetes/community)

[Kubernetes GitHub Repository](https://github.com/kubernetes/kubernetes)

[Kubernetes Blog](https://kubernetes.io/blog/)

































