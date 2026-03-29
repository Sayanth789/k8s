# Kubernetes Notes

> ⚠️ Note: I use `k` as an alias for `kubectl` in all commands.

---

## Node and Pod

- **Node**: Virtual or physical machine  that runs Kubernetes workloads.
  - Each node runs pods and has components like kubelet, container runtime, and kube-proxy.
  - Can be a master (control plane) or worker (executes workloads).
 
- **Pod**: Smallest deployable unit in Kubernetes, usually containing a single container or tightly coupled containers.  
  - Provides network and storage abstraction for containers.  
  - Each pod gets a unique IP address within the cluster.  
  - Pods are ephemeral; they can be created, scaled, and destroyed dynamically.

- **Service**: An abstraction that defines a logical set of pods and a policy to access them.   
  - Provides stable networking for pods.
  - Lifecycle of Pod and Service are independent allowing pods to die and be replaced without affecting clients.
  - 

- **Ingress**: Secures connections to pods.
---

## ConfigMap and Secret
- **ConfigMap**: Stores non-sensitive configuration data for pods.  
  - Can be injected as environment variables, command-line arguments, or files.  
  - Helps separate configuration from application code.

- **Secret**: Stores sensitive data like passwords, API keys, or tokens in Base64 encoding.  
  - Can also be injected as environment variables or mounted as files.  
  - More secure than storing credentials in ConfigMaps.

---

## Data Storage

- **Volume**: A directory accessible to containers in a pod that persists beyond container restarts.  
  - Helps manage data that needs to survive pod restarts.  
  - Can be backed by local disk, network storage, or cloud volumes.

- **PersistentVolume (PV)**: Cluster-wide storage resource that admins provision or dynamically create.  
  - Represents actual physical storage.  

- **PersistentVolumeClaim (PVC)**: A request for storage by a pod.  
  - Binds to a PV that satisfies storage size, access modes, and class.

Example StorageClass:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: storage-class-name
provisioner: kubernetes.io/aws-ebs
parameters:
  type: io1
  iopsPerGB: "10"
  fsType: ext4
```

### Node Process

- Worker server; each node has multiple pods.  
- 3 processes must be installed on every node.  
- Worker nodes do the actual work.

### Master Process

- Gateway of cluster, acts as gatekeeper for authentication.  
- Handles authentication, scheduling, state management, and API requests.
### Minikube
- Tool to run a single-node Kubernetes cluster locally.
- Ideal for testing and learning purposes.
- Runs nodes inside a lightweight VM (e.g., VirtualBox, Docker).
- Creates a VirtualBox VM on local machine.  
- Node runs inside this VM for testing purposes.

---

## Config File Parts

1. **Metadata**: Can be objects name, labels, annotations  
2. **Specification**  - Desired state and configurations
3. **Status** -Current state, automatically managed by Kubernetes for self-healing. (auto-generated; supports self-healing)

---

## Namespace

- Provides logical separation of resources in a cluster.
- Helps avoid conflicts and manage staging/development.  
- Supports Blue/Green deployment.  
- Access & resource limits per namespace.
- Prevents naming collisions.
---

## Helm

-  Helm is the package manager for K8s YAML files.
-  Packages are called **charts** which can include templates, default values and dependencies.
- Supports templating & distributing charts.  
- Common charts: Elasticsearch, MySQL, MongoDB, Prometheus.

### Helm Chart Structure

```
mychart/              #Top level folder name of chart
  Chart.yaml 
  values.yaml         #Chart.yaml -> metadata info about chart   
  charts/
  templates/           #values.yaml -> values for the template files.


```

- Values injection example (`values.yaml`):

``yaml
imageName: myapp
port: 8080
version: 1.0.0
``
## Kubernetes Volumes

Needed because containers are ephemeral; data does not persist by default.
PersistentVolume (PV): cluster-wide storage resource.
PersistentVolumeClaim (PVC): request mapping to PV.
```
Example StorageClass:
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: storage-class-name
provisioner: kubernetes.io/aws-ebs
parameters:
  type: io1
  iopsPerGB: "10"
  fsType: ext4
```

## Stateful vs Statelesss Apps
* Stateless: each request is independent; use Deployments.
* Stateful: maintain state/data; use StatefulSet (databases, Elasticsearch, MongoDB).

#Services 

* ClusterIP: default, internal visibility.
* NodePort: exposes service externally via static/dynamic port.
* LoadBalancer: external load balancing.
* Use selectors and labels to route traffic to pods.

## Workloads 

* Pod: running container(s).
* ReplicaSet: ensures desired number of pods.
* Deployment: manages ReplicaSets; allows updates & rollbacks.
* DaemonSet: runs pod on all (or subset of) nodes.
* Job: executes pods to completion.
* CronJob: scheduled jobs.

## Command Cheat Sheet 
```
## Command Cheat Sheet (Extended)

# Deployment
k create deploy [name] --image=busybox --replicas=3 --port=80
k apply -f [definition.yml]
k get deploy
k describe deploy [name]
k delete deploy [name]
k rollout status deploy [name]                # Check rollout status
k rollout history deploy [name]               # View rollout history
k rollout undo deploy [name]                  # Rollback deployment
k scale deploy [name] --replicas=5           # Manually scale deployment
k get deploy [name] -o yaml                   # Get full manifest
k edit deploy [name]                          # Edit deployment live

# ReplicaSet
k get rs
k describe rs [name]
k delete rs [name]
k scale rs [name] --replicas=3               # Scale ReplicaSet
k get rs [name] -o yaml                       # Full manifest

# DaemonSet
k apply -f [definition.yml]
k get ds
k describe ds [name]
k delete ds [name]
k rollout status ds [name]
k edit ds [name]

# Job
k create job [name] --image=busybox
k apply -f [definition.yml]
k get job
k describe job [name]
k delete job [name]
k logs job/[name]                             # View logs of completed job
k get pods --selector=job-name=[name]        # List pods of a job

# CronJob
k apply -f [cronjob.yml]
k get cronjob
k describe cronjob [name]
k delete cronjob [name]
k get jobs --selector=job-name=[cronjob-name]  # See triggered jobs
k logs job/[job-name]                           # Logs from triggered job pods

# Horizontal Pod Autoscaler
k autoscale deployment [name] --cpu-percent=50 --min=3 --max=10
k get hpa [name]
k describe hpa [name]
k delete hpa [name]

# Pods (useful across all workloads)
k get pods
k describe pod [name]
k logs [pod-name]                             # Logs from a pod
k logs -f [pod-name]                          # Follow logs
k exec -it [pod-name] -- /bin/sh             # Access pod shell
k port-forward [pod-name] 8080:80            # Port-forward to local machine
k delete pod [name]                           # Force delete pod

```

# Rolling Updates 
Update deployments with minimal downtime.
Key options : 
* `maxUnavailable:` Pods allowed to be unavailable during updates.
* `maxSurge`: Pofs allowed above the desired count during update.

Example: 
```
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 25%
    maxUnavailable: 25%
```
## Blue/Green Deployment
* Run two environments: Blue (current) and Green (new).
* Switch traffic from Blue → Green after validation.
* Reduces downtime & rollback complexity.

## Labels and Selectors 
* **Labels** : Key-value pairs to categorize the objects
* **Selectors**: Filter or select resources using the labels.
* Used for services, Replicaset, Deplyments etc.

## Networking
* Pod-to-Pod communication: via Pod IPs.
* Service-to-Pod communication: via ClusterIP/NodePort/LoadBalancer.
* Ingress: Manage external HTTP/S access.
* NetworkPolicy: Control traffic flow between pods.


# Scaling 
* Horizontal Pod Autoscaling (HPA) : Scale pods based on the CPU/memory metrics.

* Manual scaling: `k scale deplyment [name] --replicaset=[num]`

* HPA requires Metric server, resources & limits defined.

##Tips & Best Practices
* Use Namespaces to isolate teams/environments.
* Always define resource limits for pods.
* Use ConfigMaps and Secrets instead of hardcoding values.
* Prefer Deployments over managing ReplicaSets manually.
* Monitor pods using k get pods -o wide or tools like k9s.
* Persist data with PVCs for databases and stateful applications.
* Use Helm charts for reproducible deployments.
  
  


  


  
