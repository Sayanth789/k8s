## 📚 K8s ⚛️ 🚢

## These are some personal notes about K8s. May be someone will find useful.
## Notes:
### About kubernetes
* **Open-source container orchestration platform, help automate the deployment, scaling and management of containers**

**Containers (like Docker) package your app and its dependencies into a single, portable unit. Kubernetes takes it further by managing many containers across multiple machines reliably.**


## 🔐 Key Features
* **Automated Scheduling & Scaling**

**Deploy containers across nodes efficiently.**

**Scale pods up/down automatically with Horizontal Pod Autoscaler.**
## **Self-Healing**
**Restarts failed containers automatically.**
**Replaces containers when nodes fail.**
**Kills unresponsive pods.**
## Service Discovery & Load Balancing
**Provides internal networking between pods.**
**Supports external traffic via Services, NodePorts, or Ingress.**
## Declarative Configuration
**Define desired state in YAML/JSON files (Deployments, Services, ConfigMaps).**
**Kubernetes ensures the live cluster matches that state.**
**Rolling Updates & Rollbacks**
**Deploy new versions without downtime.**
**Rollback to previous versions if something goes wrong.**
## Secret & Config Management
**Store sensitive data securely using Secrets.**
**Use ConfigMaps for app configuration.**
## Extensibility
**Supports custom resources, operators, and third-party integrations.**

## Common Components
**Node: A machine (physical or virtual) that runs containers.**
**Pod: The smallest deployable unit (one or more containers).**
**Deployment: Ensures a set number of pods are running.**
**Service: Stable networking endpoint for pods.**
**Ingress: Handles external HTTP/S traffic and routing.**

In short: **Kubernetes manages your containers like an operating system manages processes, making it easy to deploy, scale, and maintain apps in production.**
