# ☸️ The Ultimate Kubernetes (K8s) Guide

Welcome to the complete guide to understanding, deploying, and managing Kubernetes. Whether you are a beginner looking to grasp the basics or an engineer needing a quick command reference, this guide has you covered.

## 📖 Table of Contents
1. [What is Kubernetes?](#what-is-kubernetes)
2. [Core Use Cases](#core-use-cases)
3. [Architecture & Key Concepts](#architecture--key-concepts)
4. [Linux & Bash Setup](#linux--bash-setup)
5. [Essential `kubectl` Commands](#essential-kubectl-commands)
6. [Debugging & Troubleshooting](#debugging--troubleshooting)

---

## 🧐 What is Kubernetes?
Kubernetes (often abbreviated as **K8s**) is an open-source container orchestration platform originally developed by Google. It automates the deployment, scaling, and management of containerized applications. 

Think of containers (like Docker) as the musicians in an orchestra, and Kubernetes as the conductor ensuring everyone plays in harmony, scales when the music gets loud, and restarts if a musician drops their instrument.

---

## 💡 Core Use Cases
* **Microservices Management:** Seamlessly deploy and manage hundreds of microservices that need to communicate with each other.
* **Auto-Scaling:** Automatically spin up more application instances during traffic spikes (e.g., Black Friday sales) and scale them down to save costs when traffic drops.
* **High Availability & Self-Healing:** If a container or node crashes, K8s automatically replaces and reschedules it without human intervention.
* **Hybrid & Multi-Cloud:** Run workloads consistently across on-premises servers, AWS, GCP, and Azure.
* **Automated Rollouts/Rollbacks:** Deploy new versions of your app gradually with zero downtime. If something goes wrong, roll back instantly.

---

## 🏗 Architecture & Key Concepts

### The Control Plane (The Brain)
* **API Server:** The front end of the control plane. All communication goes through here.
* **etcd:** Consistent and highly-available key-value store used as Kubernetes' backing store for all cluster data.
* **Scheduler:** Watches for newly created Pods with no assigned node, and selects a node for them to run on.
* **Controller Manager:** Runs controller processes (e.g., Notice when nodes go down, create endpoints).

### Worker Nodes (The Muscle)
* **Kubelet:** An agent that runs on each node and ensures containers are running in a Pod.
* **Kube-Proxy:** Maintains network rules on nodes, allowing network communication to your Pods.
* **Container Runtime:** The software that is responsible for running containers (e.g., containerd, CRI-O).

### K8s Objects
* **Pod:** The smallest deployable unit. Usually contains one container (sometimes more).
* **Deployment:** Manages stateless applications, ensuring the desired number of Pods are running.
* **Service:** A stable network endpoint (IP address) to access a group of Pods.
* **Namespace:** Virtual clusters backed by the same physical cluster (great for separating Dev/Staging/Prod).
* **Ingress:** Manages external access to the services in a cluster, typically HTTP/HTTPS.

---

## 🐧 Linux & Bash Setup

To make working with K8s via CLI faster, add these essential configurations to your `~/.bashrc` or `~/.zshrc` file:

```bash
# Enable kubectl auto-completion
source <(kubectl completion bash) # For bash
# source <(kubectl completion zsh) # For zsh

# Essential Aliases (Saves thousands of keystrokes)
alias k='kubectl'
alias kg='kubectl get'
alias kd='kubectl describe'
alias kdel='kubectl delete'
alias klog='kubectl logs -f'
alias kex='kubectl exec -it'

# Auto-complete for the 'k' alias
complete -o default -F __start_kubectl k
```
Run source ~/.bashrc after adding these lines.
---

### 💻 Essential kubectl Commands

## Cluster Information
```
kubectl cluster-info                # Display addresses of the master and services
kubectl get nodes                   # List all worker nodes in the cluster
kubectl top nodes                   # Show CPU and memory usage of nodes
```


## Working with Pods
```
kubectl get pods                    # List all pods in the current namespace
kubectl get pods -A                 # List pods in ALL namespaces
kubectl get pods -o wide            # List pods with more details (IPs, nodes)
kubectl run my-pod --image=nginx    # Create a simple pod running Nginx
kubectl delete pod my-pod           # Delete a pod
```
## Deployments & Scaling

```
kubectl get deployments             # List deployments
kubectl create deployment my-dep --image=nginx  # Create a new deployment
kubectl scale deployment my-dep --replicas=5    # Scale the deployment to 5 pods
kubectl set image deployment/my-dep nginx=nginx:1.19  # Rolling update to a new image version
kubectl rollout status deployment/my-dep        # Check the status of the rollout
kubectl rollout undo deployment/my-dep          # Rollback to the previous version
```

## Services & Networking
```
kubectl get services                # List all services
kubectl expose deployment my-dep --port=80 --target-port=8080 --type=LoadBalancer # Expose a deployment
kubectl port-forward pod/my-pod 8080:80 # Forward local port 8080 to pod's port 80
```
---
### 🛠 Debugging & Troubleshooting
When things go wrong, these are your best friends:
```
# 1. Check the Pod's status and events
kubectl describe pod <pod-name>

# 2. Check the container logs (add -f to stream them live)
kubectl logs <pod-name>
kubectl logs <pod-name> -c <container-name> # If pod has multiple containers

# 3. Get inside the container to poke around
kubectl exec -it <pod-name> -- /bin/bash  # or /bin/sh

# 4. Check cluster events for broader issues
kubectl get events --sort-by='.metadata.creationTimestamp'
```


