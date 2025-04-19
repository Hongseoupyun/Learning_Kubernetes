# 📘 Kubernetes Resource Control – CPU and Memory

## 🧠 What Are Resources in Kubernetes?
In Kubernetes, resources refer to the computing power and memory that your containers need to run. The two main types of resources you control are:

- CPU: How much processing power your container can use

- Memory (RAM): How much memory your container can use

These resources are finite on any given node, so allocating them properly is essential for stability and performance.

## ⚠️ Why Resource Control Is Important

1. Prevent Node Overload

- Without limits, a single pod could consume all CPU or memory on a node.

- This can lead to other pods crashing or being evicted.

2. Ensure Fair Resource Distribution

- In multi-tenant environments or shared clusters, you need to allocate resources fairly.

3. Improve Scheduling Decisions

- Kubernetes uses resource requests to schedule pods on nodes with enough capacity.

4. Avoid Outages

- If a container uses more memory than allowed, it can get OOMKilled (Out of Memory Killed).

- Without proper CPU limits, containers may hog the CPU, causing performance issues for others.

## 🔧 Key Concepts: requests vs limits
|Setting | Description|Effect on Scheduling|Effect at Runtime|
|:------|:---|:---|:---|		
|```requests```	|The minimum amount of CPU/memory guaranteed|	Used for pod placement	|Container can exceed this|
|```limits```|	The maximum amount a container is allowed to use|	Not used for placement	|Container is throttled or killed if exceeded|

🧮 CPU Unit Example:
- 1 CPU = 1 vCPU/core

- 0.5 CPU = 500 millicores (half a core)

🧮 Memory Unit Example:
- 512Mi = 512 Mebibytes

- 1Gi = 1 Gibibyte

📄 Example Resource Configuration in a Pod
```
apiVersion: v1
kind: Pod
metadata:
  name: resource-demo
spec:
  containers:
  - name: app-container
    image: my-app:latest
    resources:
      requests:
        memory: "256Mi"
        cpu: "250m"
      limits:
        memory: "512Mi"
        cpu: "500m"
```
- `requests`: Kubernetes will reserve 250m CPU and 256Mi memory for scheduling this pod.

- `limits`: The container cannot exceed 500m CPU or 512Mi memory at runtime.

## 🧨 What Happens If You Exceed Limits?
- Memory exceeded: Container is terminated and marked as OOMKilled (Out of Memory Killed)

- CPU exceeded: Container is throttled, not killed — it just runs slower

✅ Best Practices
- Always set both requests and limits.

- Tune based on actual usage — use monitoring tools like Prometheus + Grafana.

- Set conservative requests to allow better pod density.

- Avoid overcommitting memory — unlike CPU, memory cannot be throttled.