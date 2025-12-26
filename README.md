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
If we want we can set namespace context for all future kubectl commands use bellow commands
```
kubectl config set-context --current --namespace=<namespace name>
```
If you want to create a pod in particular namespace follow bellow command. if you donot use the namespace other wise it use the default namespace
```
kubectl apply -f <file name> --namespace=<namespace name>
```
If you want to delete a namespace use bellow command
```
kubectl delete namespace <namespace-name>
```

---
## Secret and config file management
In Kubernetes, Secrets and ConfigMaps are two essential resources used for managing configuration and sensitive data separately from application code. They provide a way to decouple configuration from the application itself, making it easier to maintain and secure, especially in production environments.
### Secrets in Kubernetes
**Secrets** are designed to store sensitive data, such as passwords, tokens, certificates, or API keys. Kubernetes provides mechanisms to encode and manage these secrets securely, limiting their exposure and ensuring they are only accessible to authorized entities.
**Key Points about Secrets:**
- **Sensitive Data:** Secrets are used for storing sensitive information that should not be exposed in plain text, like passwords, authentication tokens, SSH keys, etc.
- **Base64 Encoding** Although the data in Secrets is stored in base64-encoded format, it is still accessible to users who have permission to access the secret. Kubernetes does not encrypt secrets by default (but you can configure encryption at rest).
- **Access Control:** Kubernetes allows fine-grained access control to Secrets using RBAC (Role-Based Access Control), ensuring that only authorized users or services can access secrets.
- **Types of Secrets:**
  - **Opaque:** Generic secret type used for arbitrary key-value pairs.
  - **DockerConfig:** Stores Docker credentials for accessing private Docker registries.
  - **TLS:** Stores TLS certificates and keys.
  - **BasicAuth:** Stores HTTP basic authentication credentials.
  - **SSHAuth:** Stores SSH authentication information.

### ConfigMaps in Kubernetes
**ConfigMaps** are used for storing non-sensitive configuration data in a Kubernetes cluster. These configurations might include settings, environment variables, configuration files, or any data that does not require high security.
**Key Points about ConfigMaps:**
- **Non-Sensitive Data:** ConfigMaps store configuration data like URLs, application settings, or any other non-sensitive information that needs to be accessible by Pods.
- **Text-Based:** ConfigMaps store data as plain text key-value pairs. There is no base64 encoding, as the data is not sensitive.
- **Use Cases:** ConfigMaps are ideal for managing environment-specific settings (e.g., staging, production), application configurations, or feature flags that need to be applied across multiple Pods.
- **Access Control:** While ConfigMaps are generally not sensitive, Kubernetes still supports RBAC for controlling access to ConfigMaps.
**Key Differences Between Secrets and ConfigMaps**
  
| Feature    | Secrets                                    | ConfigMaps                                          |
|------------|--------------------------------------------|-----------------------------------------------------|
| **Purpose**  | Store sensitive data (passwords, API keys, certificates, etc.) | Store non-sensitive configuration data (URLs, settings, environment variables) |
| **Data Encoding** | Data is base64-encoded (but not encrypted by default) | Data is stored as plain text key-value pairs        |
| **Use Cases** | Storing sensitive information like passwords, certificates, OAuth tokens | Storing application settings, environment variables, feature flags |

### How To Create a Secret file
You can create secret file many way like using yaml configuration or you can use inline kubectl command. I will use yaml configuration
Bellow I add a sample demo secret yaml Format to create a Secret
```
apiVersion: v1
kind: Secret
metadata: 
  name: mongo-secret
type: Opaque
data:
  MONGO_USER: bW9uZ291c2Vy
  MONGO_PASSWORD: bW9uZ29wYXNzd29yZA==
```
One thing we have to keep in our mind that is we will keep our secret value in **base64** encode.
Now we run bellow command to create this Secrets in a specific namespace
```
kubectl apply -f <file-directory> --namespace=<namespace-name>
```
We can validatate our secret using bellow command
```
kubectl get secret --namespace=<namespace name>
```
If we want to see spcecific secret all value we can use bellow command
```
kubectl get secret <secret-name> --namespace=<namespace-name> -o yaml
```
If we want, we also delete this secret using bellow command
```
kubectl delete secret <secret-name> --namespace=<namespace-name>
```

### How to create a ConfigMap in a particular Namespace
As like the secret fule you also can create ConfigMap. we will use yaml format here
Bellow I add a sample demo ConfigMap yaml format to create a ConfigMap
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongo-config 
data:
  MONGO_URL: anything # we will set later
```
Now we run bellow command to create this ConfigMap in a particular namespace
```
kubectl apply -f <file-directory> --namespace=<namespace-name>
```
After creating the ConfigMap you can validate using bellow command
```
kubectl get configmap --namespace=<namespace-name>
```
If we want to see particular configmap yaml file we can use bellow command
```
kubectl get configmap <configmap-name> --namespace=<namespace-name> -o yaml
```
If we want to delete a particular configmap we can use bellow command
```
kubectl delete configmap <configmap-name> --namespace=<namespace-name>
```

---

## Pods
A **Pod** is the smallest and simplest unit of deployment in Kubernetes. It represents a single instance of a running process in your cluster. When you deploy an application in Kubernetes, it runs inside a Pod.
### What is a Pod?
A **Pod** is a **logical host** for one or more containers that share the same network, storage, and runtime environment. In Kubernetes, containers run inside Pods. Pods can host:
- **A single container:** This is the most common use case.
- **Multiple containers:** Sometimes you may need more than one container inside the same Pod, which may need to share resources, such as data volumes or network connections.

### Why Use Pods?
**Pods** provide an abstraction layer between your application containers and the underlying infrastructure. They are designed to:
- **Simplify Deployment:** By grouping containers into logical units.
- **Enable Shared Resources:** Allow containers to share resources like storage volumes and network interfaces.
- **Improve Scalability and Management:** Pods allow Kubernetes to manage and scale containers effectively, automatically adjusting the number of replicas as required.

Bellow I add a demo structure how to create a pod using yaml file in a particular namespace:
```
apiVersion: v1
kind: Pod
metadata:
  name: next-app-pod
spec:
  containers:
  - name: next-app
    image: noyonsaha/docker-next-app:latest
    ports:
      - containerPort: 3000
```
Now create the pod using bellow command:
```
kubectl apply -f <file-directroy> --namespace=<namespace-name>
```
Now you can validate using bellow command that pod is created or not
```
kubectl get pod --namespace=<namespace-name>
```
If we want to see the pod information in wide format we can use bellow command:
```
kubectl get pod --namespace=<namespace-name> -o wide
```
If we want to describe a pod or want to know more details in the pod we can use bellow command
```
kubectl describe pod <pod-name> --namespace=<namespace-name>
```
If we want to see the logs of this pod we can use bellow command
```
kubectl logs <pod-name> --namespace=<namespace>
```
If the Pod has multiple containers, you can specify the container name using bellow command
```
kubectl logs <pod-name> -c <container-name> --namespace=<namespace>
```
If we want to delete a pod we can use bellow command
```
kubectl delete pod <pod-name> --namespace=<namespace>
```
