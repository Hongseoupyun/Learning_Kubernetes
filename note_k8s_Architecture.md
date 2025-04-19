![image](https://github.com/user-attachments/assets/fa0805fa-c246-4d50-bebb-3cb370e8799c)


# 📘 Kubernetes Architecture – Study Notes

## 🧱 Overview: What is a Kubernetes Cluster?
A Kubernetes cluster is made up of two main parts:

1. Control Plane – the "brain" of the cluster

2. Worker Nodes – where applications (pods) actually run

If Kubernetes were an airport:

- The control plane is the air traffic control tower 🛫

- Worker nodes are the gates and runways 🛬

## 🧠 Control Plane Components
These manage the overall state and behavior of the cluster:


|Component|Role|
|:------|:---|
|kube-apiserver|	Exposes the Kubernetes API (RESTful). Central communication hub between users, controllers, and components. All requests (e.g., kubectl) go through here.
|etcd|Distributed key-value store. Stores the entire cluster state. Only the API server communicates directly with it.|
|kube-scheduler	|Assigns newly created pods to available nodes based on resources, constraints, and policies.|
|kube-controller-manager|	Continuously monitors the cluster and ensures the actual state matches the desired state. Examples: node controller, replication controller.|
|cloud-controller-manager|	Integrates Kubernetes with your cloud provider (e.g., AWS, GCP). Manages cloud-specific resources like load balancers or volumes.|

💡 In managed services like GKE or EKS, the control plane is hidden from you — your cloud provider manages and maintains it.

## ⚙️ Worker Node Components
Worker nodes actually run your containers inside pods:

|Component|Role|
|:------|:---|
|kubelet	|Agent running on each node. Ensures containers are running as expected. Talks directly to the API server.|
|container runtime	|Pulls images and runs containers. Common options: containerd, CRI-O, Kata. Docker is no longer supported directly since v1.24 (Dockershim removed).|
|kube-proxy|	Maintains networking rules. Enables communication between services and pods across nodes. Handles routing and forwarding.|

## 🕓 Pod Creation – Sequence of Events
Here’s how the control plane and a node work together to create a pod:

1. You run: `kubectl apply -f deployment.yaml`

2. `kubectl` sends the request to the kube-apiserver

3. The API server stores the deployment spec in etcd

4. The controller manager notices the new deployment

5. The scheduler assigns the pod to a node

6. The assignment is updated in etcd

7. The kubelet on the selected node fetches the pod spec

8. The container runtime pulls the image and runs the container

9. The kubelet reports back pod health/status to the API server

10. etcd is updated with the new state

## 🧩 Key Takeaways
- The API server is the heart of communication.

- etcd is the single source of truth for the entire cluster’s state.
 
- Worker nodes do the real work (run pods), but are completely managed by the control plane.

- Everything is containerized — even the control plane components are pods themselves (in kube-system namespace).

- The architecture is designed for resilience, scalability, and automation.

