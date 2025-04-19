# 🧪 Lab Notes – Exposing a Service via minikube tunnel
## ✅ Objective
Simulate external access to a Kubernetes service running on Minikube by using minikube tunnel and exposing a Deployment via a LoadBalancer service.

## 🛠️ Steps Performed
Started the tunnel to allow external access:

```
minikube tunnel
```
This command enables Minikube to simulate a cloud LoadBalancer by assigning an external IP (e.g., 127.0.0.1) that routes to the service inside the cluster.

Applied the following Service definition:
```
apiVersion: v1
kind: Service
metadata:
  name: demo-service
  namespace: development
spec:
  selector:
    app: pod-info
  ports:
    - port: 80
      targetPort: 3000
  type: LoadBalancer
```
## 📄 Explanation of Key Concepts
type: LoadBalancer
Tells Kubernetes to expose the service externally.

In a cloud environment, this would provision a real external IP.

In Minikube, this is simulated through minikube tunnel, which maps the service to 127.0.0.1.

## 📊 Output of kubectl get svc -n development
```
NAME           TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
demo-service   LoadBalancer   10.xxx.xxx.xxx   xxx.0.x.x     xx:xxxxx/TCP   21m 
```
🔹 CLUSTER-IP
10.xxx.xxx.xxx: Internal IP used only inside the cluster.

Other pods or services within the same cluster can communicate with this IP.

🔹 EXTERNAL-IP
xxx.0.x.x  : The external IP exposed by minikube tunnel.

From the local machine (browser or curl), you can access the service like this:

```
curl http://xxx.0.x.x
```
This simulates how the service would be accessed in a real cloud environment.

## 🔹 PORT(S)
xx:xxxxx/TCP:

Externally, the service listens on port 80.

Internally, it maps to a NodePort (in this case, 31973) managed by Kubernetes.

## ✅ Summary
The LoadBalancer service type works seamlessly with minikube tunnel to provide local access to internal cluster services.

This setup is especially useful for testing external access locally, without deploying to a cloud provider.

Understanding CLUSTER-IP vs EXTERNAL-IP is crucial for proper service exposure and debugging.

