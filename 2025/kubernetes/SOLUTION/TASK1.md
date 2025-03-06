## Task 1: Understand Kubernetes Architecture & Deploy a Sample Pod

## Scenario:
Familiarize yourself with Kubernetes’ control plane and worker node components, then deploy a simple Pod manually.

## Steps:
## 1️⃣ Study Kubernetes Architecture:

The primary components of a Kubernetes cluster are a Control Plane and worker machines known as nodes. At least one worker node is present in a cluster. The Control Plane controls the Worker Nodes, and the Kubectl CLI interacts with it.

#### Kubernetes – Cluster Architecture

Kubernetes follows a client-server architecture with a master node on a single Linux system and multiple worker nodes on Linux workstations. The master node includes the API server, controller manager, scheduler, and etcd for cluster state storage. Worker nodes run a container runtime (e.g., Docker), kubelet for communication with the master, and kube-proxy for networking.

### Kubernetes Components
Kubernetes is made up of several components, each of which serves a specific role in the overall system. These components can be classified into two categories:

### 1. Control plane (Master Node)

In Kubernetes, the control plane is responsible for managing the cluster, ensuring the desired state of the system, scheduling workloads, and maintaining overall cluster health.

#### Control Plane Components

##### 1. API server (kube-apiserver)
The API Server is the gateway to the Kubernetes cluster, handling all requests from tools like kubectl. It validates and processes requests, ensuring they follow security rules before forwarding them to the appropriate components. No direct access to the cluster is allowed—everything must go through the API Server.

##### 2. Scheduler or Kube-Scheduler
When the API Server gets a request to schedule a pod, it passes it to the Scheduler. The Scheduler analyzes resource availability and decides the best worker node for the pod to ensure efficient cluster performance.

##### 3. Controller-Manager or Kube-Controller-Manager
The kube-controller-manager runs controllers that manage the cluster's state. It ensures the right number of pod replicas are running (Replication Controller) and tracks node health (Node Controller) to mark them as "Ready" or "Not Ready."

##### 4. etcd (Key-Value Store)
etcd is the cluster's key-value store, storing all state changes. It acts as the cluster's brain, informing the Scheduler and other components about resource availability and updates.

### 2. Worker Node

Worker nodes run the actual workloads. Each node hosts multiple pods, which contain containers. Three key processes manage pods on a node: kubelet (node agent), kube-proxy (networking), and container runtime (runs containers).

#### Key Components of Worker Nodes:
#### 1. Container Runtime
A container runtime runs application containers inside pods. It handles container execution, management, and isolation. Example: Docker, containerd, or CRI-O.

#### 2. kubelet
Kubelet runs on each node and manages pods. It interacts with the container runtime to start containers and ensures they run as expected on the node.

#### 3. kube-proxy

kube-proxy handles network traffic in a cluster. It forwards requests from Services to the right pods in a worker node, ensuring proper communication and load balancing.

#### 4. Pod
A Pod is the smallest unit in Kubernetes. It can have one or multiple containers that share the same network and storage, allowing them to communicate and work together efficiently.

## 3. Kubernetes Networking

Kubernetes networking enables communication between different components:

- ***Pod-to-Pod Communication*** : All pods in a Kubernetes cluster can communicate with each other, even across different nodes. This is enabled by CNI (Container Network Interface) plugins like Flannel, Calico, or Cilium, which manage networking and ensure seamless pod-to-pod communication.
- ***Pod-to-Service Communication*** : Services provide stable IPs and DNS names for load balancing across pods.
- ***Ingress Controller*** : Manages external access (HTTP/HTTPS) using Ingress resources.


### Kubernetes Objects
These objects define desired states and configurations.

- ***Pods*** – Smallest deployable unit.
- ***ReplicaSet*** – Ensures that a specified number of pod replicas are always running, automatically creating or deleting pods to match the desired count.
- ***Deployment*** – Manages rolling updates for ReplicaSets, ensuring smooth updates by gradually replacing old pods with new ones while    maintaining availability.
- ***StatefulSet*** – Used for stateful applications like databases. It ensures stable pod identities, persistent storage, and ordered scaling for reliable state management.
- ***DaemonSet*** – Ensures that a specific pod runs on every node in the cluster, commonly used for logging, monitoring, or networking services.
- ***Service*** – Exposes pods either internally within the cluster or externally to users, enabling stable network access despite pod restarts or changes.
- ***ConfigMap & Secret*** – Store configuration and sensitive data.
- ***Persistent Volume (PV) & Persistent Volume Claim (PVC)*** – manage storage in Kubernetes, providing durable and reusable storage for pods.


###  Kubernetes Workflow
- Developer creates a Deployment manifest (YAML).
- kubectl sends the request to the API Server.
- Scheduler assigns the workload to a Worker Node.
- Kubelet ensures that the Pod runs correctly.
- Kube Proxy manages networking and load balancing.
- Controllers maintain the desired state.

---
## 2️⃣ Deploy a Sample Pod
Create a YAML file (e.g., pod.yaml) to deploy a simple Pod (such as an NGINX container).

    apiVersion: v1
    kind: Pod
    metadata:
     name: nginx-pod
     labels:
        app: nginx
    spec:
     containers:
        - name: nginx
        image: nginx:latest
        ports:
            - containerPort: 80

### Apply the YAML using

    kubectl apply -f pod.yaml

This command creates an ***NGINX pod*** inside your Kubernetes cluster.

### Verify the Deployment

    kubectl get pods

If the pod is not running, check its status using:

    kubectl describe pod nginx-pod

------

## 3️⃣ Interview Questions & Answers

### 1️⃣ How do Kubernetes control plane components work together?

- ***API Server*** –  The central hub of Kubernetes, handling all requests (kubectl, UI, automation) and coordinating communication between control plane components and worker nodes. It ensures cluster state consistency by interacting with etcd and other controllers.
- ***Controller Manager*** – Continuously monitors the cluster state and runs controllers to maintain the desired state. It handles tasks like scaling, node health checks, and managing replicas by reconciling the actual state with the desired state.
- ***Scheduler*** – Selects the best worker node for each Pod based on resource availability, constraints, and policies. It ensures balanced workload distribution and efficient resource utilization.
- ***etcd*** – Is a distributed key-value store that holds Kubernetes cluster data, including configurations, node states, and secrets. It ensures consistency and reliability for the control plane. 
- ***Cloud Controller Manager*** – Integrates Kubernetes with cloud provider APIs, managing cloud-specific resources like load balancers, networking, and storage. It separates cloud-dependent logic from the core Kubernetes system. 

Each worker node reports back to the control plane, and Kubelet ensures that pods are running correctly. 

### 2️⃣ What is the role of etcd in Kubernetes?
- etcd is a key-value store that manages and replicates Kubernetes cluster state, ensuring consistency and availability.
- etcd is a reliable, distributed system that stays highly available and updates itself using a "watch" function. 
- It stores desired state (what the cluster should look like) and actual state (what it currently looks like).
- The API server is the only component that communicates directly with etcd.
- A Raft-based consensus mechanism ensures that updates to etcd are fault-tolerant.

#### NOTE
If etcd is not set up properly, it can fail and bring down the whole Kubernetes cluster. Also, since it stores all cluster data, a security breach could expose sensitive information, making it a key target for attackers. ***(Always back up etcd!)***

### 3️⃣ If a Pod fails to start, how would you diagnose the issue?

Here are some troubleshooting steps:

- ####  Check pod status
        kubectl get pods

- #### Describe the Pod
     
        kubectl describe pod <pod-name> -n <namespace>

- #### Inspect Pod Logs

        kubectl logs <pod-name> -n <namespace>

- #### Check Kubernetes Events

        kubectl get events

- ####  Check node status

        kubectl get nodes

- ####  Inspect Deployment YAML

        kubectl get pod <pod-name> -o yaml


------

## Final Things to Take Care Of in Kubernetes 

- **Monitor & Logging**: Use tools like Prometheus, Grafana, and ELK to track cluster health.
- **Backup etcd**: Regularly take snapshots to prevent data loss.
- **Security Best Practices** Use RBAC, Network Policies, and TLS to secure the cluster.
- **Resource Limits**: Set CPU/memory limits to prevent pods from overloading nodes.
- **Autoscaling**: Enable Horizontal Pod Autoscaler (HPA) & Cluster Autoscaler for efficiency.
- **Probes & Health Checks**: Configure Liveness & Readiness probes to auto-recover failing pods.
- **Use Namespaces**: Organize workloads and apply resource quotas.
- **Network Policies**: Restrict pod communication for security.
- **Regular Updates**: Keep Kubernetes, nodes, and dependencies up to date.
- **Disaster Recovery Plan**: Have a tested recovery strategy for failures.

Following these ensures a stable, secure, and scalable Kubernetes environment!












