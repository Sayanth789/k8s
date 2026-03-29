# Kubernetes Project - k8s

> ⚠️ Note: I use `k` as an alias for `kubectl` in all commands.

---

## Kubernetes Overview 🌐

**Kubernetes (k8s)** is an **open-source container orchestration platform** that automates the deployment, scaling, and management of containerized applications. Containers package your app and its dependencies, and Kubernetes manages these containers reliably across multiple nodes.

### Key Features

- **Automated Scheduling & Scaling**  
  Deploy containers across nodes efficiently. Scale pods automatically with **Horizontal Pod Autoscaler (HPA)**.

- **Self-Healing**  
  Restarts failed containers, replaces pods on node failure, and kills unresponsive pods.

- **Service Discovery & Load Balancing**  
  Provides internal networking between pods, and external traffic management via Services or Ingress.

- **Declarative Configuration**  
  Define desired state in YAML/JSON files. Kubernetes ensures the cluster matches that state.

- **Rolling Updates & Rollbacks**  
  Deploy new versions without downtime and rollback if needed.

- **Secrets & Config Management**  
  Securely store sensitive data with Secrets, and app configurations with ConfigMaps.

- **Extensibility**  
  Supports custom resources, operators, and third-party integrations.

### Common Components
Find more about these from here ['README.md`](https://github.com/Sayanth789/k8s/blob/main/docker%20and%20K8s%20commands/README.md)
- **Node**: A physical or virtual machine running pods.  
- **Pod**: The smallest deployable unit; contains one or more containers.  
- **Deployment**: Manages a set number of pods and updates them safely.  
- **Service**: Stable networking endpoint for pods.  
- **Ingress**: Manages external HTTP/S traffic and routing.

---

## How to Use This Repo

This repo demonstrates Kubernetes basics including **pods, deployments, services, and scaling**.
- This file[`nginx.yml`](https://github.com/Sayanth789/k8s/blob/main/nginx.yaml) lets you access your Nginx pods from outside the cluster or internally via a stable IP.
- It ensures desired state for the pods.
- Replicas: 3 → Runs 3 pods of Nginx.
- Selector & Labels → Ties pods to this deployment and Service.
- **Container spec:**
    Image: nginx → pulls the official Nginx image.
    Limits: Restricts CPU and memory per pod.
    Ports: Exposes port 80 inside each pod.
In short: This Deployment runs, manages, and updates 3 Nginx pods, and ensures if one crashes, another will start automatically.

### Declarative Approach (YAML)

- The file [`deployment.yml`](https://github.com/Sayanth789/k8s/blob/main/deployment.yml) defines your desired state.  
- Using YAML manifests like this is called the **declarative way**: you describe **what you want**, and Kubernetes ensures the cluster matches it.

```bash
# Apply the declarative deployment
k apply -f deployment.yml

# Check resources
k get deploy
k get pods
k get svc
