## What is Kubernetes?

Kubernetes is an open-source container orchestration platform used to automate the deployment, scaling, and management of containerized applications. It provides a robust framework for managing applications in production environments, ensuring high availability, scalability, and resiliency.

## Why use Kubernetes instead of managing containers manually?

In production environments, we need to efficiently manage containers that run applications and ensure minimal downtime. Without Kubernetes, the manual process of deploying and updating containers could look like this:

1. **Shutting down the container:** You manually stop the running container.
2. **Updating the code:** You modify the codebase, and then rebuild the Docker container.
3. **Rebuilding and restarting the container:** You build the updated Docker image and start a new container with the new image.

This process creates **downtime** — the application is not accessible during the rebuild and restart, which can be a significant issue, especially in production. This downtime is problematic because, in the software industry, we aim for **zero downtime** — meaning our applications should always be available, even during updates.

### Real-Life Example: E-Commerce Platform

Imagine you're managing an e-commerce platform during a holiday sale. If you manually shut down the container to update the system, your users will experience downtime, possibly leading to missed sales and a bad user experience. Kubernetes addresses this issue by managing rolling updates, ensuring the application remains available while updating.

## How does Kubernetes solve this problem?

Kubernetes automates and manages the deployment of containers, helping you avoid downtime during updates. It provides:

- **Self-healing**: Automatically restarts containers if they fail.
- **Scaling**: Increases or decreases the number of running containers based on traffic demands.
- **Load balancing**: Distributes traffic efficiently across containers to ensure no single container gets overwhelmed.
- **Rolling updates and rollbacks**: Allows for seamless updates without downtime and the ability to roll back if something goes wrong.

In essence, Kubernetes acts as the brain that manages the deployment of your containers and ensures that your system runs smoothly without manual intervention.

## How to install Kubernetes?

To get started with Kubernetes on your local machine, you'll first need to install **kubectl**, a command-line tool used to interact with your Kubernetes cluster. Kubectl is similar to how Node.js interacts with JavaScript code. While Node.js is the runtime for JavaScript, kubectl is the tool that interacts with Kubernetes to manage your clusters.

### Step 1: Install kubectl

**Kubectl** is the command-line tool used to interact with your Kubernetes cluster. It is required to execute commands to deploy and manage applications in your Kubernetes environment. To install kubectl on your system, follow the appropriate guide for your operating system:

- **Install and Set Up kubectl on Linux:**  
  [Kubernetes Documentation for Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
  
- **Install and Set Up kubectl on macOS:**  
  [Kubernetes Documentation for macOS](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/)
  
- **Install and Set Up kubectl on Windows:**  
  [Kubernetes Documentation for Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)

Once kubectl is installed, you can run Kubernetes commands to interact with clusters.

### Step 2: Set up Kubernetes on Your Local Machine

To run Kubernetes locally, you need a tool that sets up a local cluster on your machine. Some commonly used tools include:

- **Minikube**: A tool that creates a single-node Kubernetes cluster on your local machine, allowing you to run and test Kubernetes workloads locally.
- **K3s**: A lightweight version of Kubernetes designed for IoT and edge devices, but it works well for local development environments as well.
- **Kind (Kubernetes in Docker)**: A tool that runs Kubernetes clusters in Docker containers, making it perfect for testing and development environments.

These tools will help you create a local cluster to test and manage Kubernetes deployments without needing a full-fledged cloud infrastructure. Let's look at how you can install and run a Kubernetes cluster with **Minikube** as an example:


In this particular documentation we will use **Kind**.

First of all we will show the commands then we will clear about each concepts
---
## Create a Kubernetes cluster
First of all before create a cluster we have to check all the existing cluster. So to check the all existing cluster run the bellow command
```
kind get clusters
```
Now you can see all the clusters in your machine.

If we don't have any clustes yet we need to create a clusters, so to create a clusters we need to run bellow command
```
kind create cluster --name dev-env --image kindest/node:v1.34.0
```
Using this commad we can create a kubernetes cluster. here we provde a cluster name using **--name** flag also use the specific version of kind image.

All right now we are create our first kubernetes clustes. 

If we have multiple cluster we have to navigate each clusters. To navigate each clusters we have to know what clusters we are in curretly. 
To check current cluster we can run bellow command
```
kubectl config current-context
```
or we can use bellow command
```
 kubectl config get-contexts
```
using this command we can see our current cluster name.
Now if we want to shif another cluster simply we will write the bellow command 
```
kubectl config use-context <cluster name>
```
If we need to delete a cluster we can use bellow command
```
kind delete cluster --name <cluster-name>
```
---
## Namespace
In Kubernetes, a namespace is a logical partition within a cluster that allows you to divide and organize resources like pods, services, deployments, and other objects. Namespaces help in managing resources, especially when there are multiple teams, projects, or environments using the same Kubernetes cluster.
To create namespace we can use bellow command
```
kubectl create namespace <name>
```
We can see all the namespace using bellow command
```
kubectl get namespaces
```
