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


mychart/              Top level folder name of chart
  Chart.yaml 
  values.yaml         Chart.yaml -> metadata info about chart   
  charts/
  templates/           values.yaml -> values for the template files.     


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
# Deployment
k create deploy [name] --image=busybox --replicas=3 --port=80
k apply -f [definition.yml]
k get deploy
k describe deploy [name]
k delete deploy [name]

# ReplicaSet
k get rs
k describe rs [name]
k delete rs [name]

# DaemonSet
k apply -f [definition.yml]
k get ds
k describe ds [name]
k delete ds [name]

# Job
k create job [name] --image=busybox
k apply -f [definition.yml]
k get job
k describe job [name]
k delete job [name]

# CronJob
k apply -f [cronjob.yml]
k get cronjob
k delete cronjob [name]

# Horizontal Pod Autoscaler
k autoscale deployment [name] --cpu-percent=50 --min=3 --max=10
k get hpa [name]

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
* Horizonalt Pod Autoscaling (HPA) : Scale pods based on the CPU/memory metrics.

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
  
  


  


  
