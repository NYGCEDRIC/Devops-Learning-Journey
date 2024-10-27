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












