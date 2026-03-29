# Kubernetes Notes

> ⚠️ Note: I use `k` as an alias for `kubectl` in all commands.

---

## Node and Pod

- **Node**: Virtual or physical machine.  
- **Pod**: Smallest unit in Kubernetes. Provides abstraction over containers.  
  - Usually 1 application per pod.  
  - Each pod gets its own IP address.  

- **Service**: Static or permanent IP address attached to a pod.  
  - Lifecycle of Pod and Service are independent.  

- **Ingress**: Secures connections to pods.

---

## ConfigMap and Secret

- **ConfigMap**: External configuration of applications.  
- **Secret**: Store sensitive data in Base64 format.

---

## Data Storage

- **Volume**: Attaches physical storage, can be remote or outside the cluster.  

### Node Process

- Worker server; each node has multiple pods.  
- 3 processes must be installed on every node.  
- Worker nodes do the actual work.

### Master Process

- Gateway of cluster, acts as gatekeeper for authentication.  

### Minikube

- Creates a VirtualBox VM on local machine.  
- Node runs inside this VM for testing purposes.

---

## Config File Parts

1. **Metadata**  
2. **Specification**  
3. **Status** (auto-generated; supports self-healing)

---

## Namespace

- Group resources logically.  
- Helps avoid conflicts and manage staging/development.  
- Supports Blue/Green deployment.  
- Access & resource limits per namespace.

---

## Helm

- Package manager for K8s YAML files.  
- Supports templating & distributing charts.  
- Common charts: Elasticsearch, MySQL, MongoDB, Prometheus.

### Helm Chart Structure

 mychart/
|_
|
