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

Setting Up a Kubernetes Cluster

Minikube: Set up a single-node Kubernetes cluster for testing.
````bash
minikube start
````
Working with kubectl

Get Cluster Information:
````bash
kubectl cluster-info
````
Get All Nodes in the Cluster:
````bash
kubectl get nodes
````



