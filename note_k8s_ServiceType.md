# Kubernetes Service Types: Visual and YAML Guide

🎯 Overview

Kubernetes Service helps expose Pods to the network inside and/or outside of the cluster. The type field in a Service defines how the Service is exposed.

📦 Types of Kubernetes Services

## 1️⃣ ClusterIP (default)

- 🔒 Internal only: Exposes the Service on a cluster-internal IP.

- 🧭 Used for internal communication between services.

- ❌ Cannot be accessed from outside the cluster.

YAML Example:
```
apiVersion: v1
kind: Service
metadata:
  name: internal-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```

## 2️⃣ NodePort

- 🌍 Exposes the Service on each Node's IP at a static port (default: 30000–32767).

- 🟢 Can be accessed from outside the cluster using NodeIP:NodePort.

- ✅ Useful for basic external access during development/testing.

YAML Example:
```
apiVersion: v1
kind: Service
metadata:
  name: nodeport-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30036
  type: NodePort
```
## 3️⃣ LoadBalancer

- ☁️ Works with cloud providers (e.g., GCP, AWS, Azure).

- 🚪 Automatically provisions an external load balancer and exposes the service.

- 🧠 Useful for production-level internet exposure.

YAML Example:
```
apiVersion: v1
kind: Service
metadata:
  name: lb-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```
## 4️⃣ ExternalName

- 🔗 Maps the Service to a DNS name outside the cluster.

- 🔍 No proxying; just a CNAME redirection.

YAML Example:
```
apiVersion: v1
kind: Service
metadata:
  name: external-service
spec:
  type: ExternalName
  externalName: api.external.com
```
## 📊 Summary Table

|Service Type|Exposes To|Needs Cloud?|Use Case|
|:------|:---|:---|:---|
|ClusterIP|Internal only|❌ No|Internal service-to-service access|
|NodePort|External (Node IP)|❌ No|Quick external access via node ports|
|LoadBalancer|External (Cloud IP)|✅ Yes|Production internet-facing services|
|ExternalName|DNS outside cluster|❌ No|Redirect to an external endpoint|

## ✅ When to Use What?

Choose ClusterIP if your service is only consumed internally.

Choose NodePort if you need manual access from outside without a cloud LB.

Choose LoadBalancer in cloud environments for external access.

Choose ExternalName to redirect requests to an external service.
