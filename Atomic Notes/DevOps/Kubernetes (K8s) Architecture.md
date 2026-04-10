---
status: seedling
tags: [devops, kubernetes, orchestration]
created: 2026-04-10
updated: 2026-04-10
---

# Kubernetes (K8s) Architecture

## Summary
An open-source container orchestration system for automating application deployment, scaling, and management.

## Details
Kubernetes (K8s) is the industry standard for managing containerized applications at scale, providing high availability, scaling, and orchestration across a cluster of nodes.

### Cluster Architecture
- **Control Plane**: Coordinates the cluster operations.
  - **API Server**: The gateway for all commands (`kubectl`).
  - **Scheduler**: Determines which node a pod will run on.
  - **etcd**: Consistent, distributed key-value store for all cluster data.
  - **Controller Manager**: Manages background control loops (e.g., ReplicationController).
- **Worker Nodes**: Execute the applications.
  - **Kubelet**: Agent that communicates with the control plane to ensure containers run in a pod.
  - **Kube-proxy**: Implements networking rules (e.g., iptables) for internal and external traffic.
  - **Pods**: The fundamental deployment unit, encapsulating one or more containers sharing a network and storage.

### Deployment Features
- **Rolling Updates**: Zero-downtime strategy that replaces pods incrementally.
- **Rollbacks**: If an update fails, K8s can revert to a previous state:
  ```bash
  kubectl rollout undo deployment/web-app
  ```
- **Horizontal Pod Autoscaling (HPA)**: Dynamically scales the number of pods based on CPU/Memory usage thresholds (e.g., scale up when CPU > 70%).

### Kubernetes Resource Manifests
Resources are defined using YAML. A **Deployment** manages a set of identical pods, while a **Service** provides a stable network endpoint.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:latest
---
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  selector:
    app: web
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

### Service Types
- **ClusterIP**: Internal-only access (default).
- **NodePort**: Static port on every node for external access.
- **LoadBalancer**: Integrates with cloud provider load balancers for external traffic.

### Helm (Package Manager)
Helm manages complex K8s applications using **Charts**:
- `Chart.yaml`: Chart metadata.
- `values.yaml`: Configurable parameters.
- `templates/`: Manifest templates that interpolate values from `values.yaml`.

## Associative Trails
K8s is the industry standard for managing containerized applications at scale. This note exists to document the core architecture and resource models, refining the transition from single-host [[Docker and Containerization]] to cluster-wide orchestration.

## Connections
- [[DevOps Engineering (MOC)]]
- [[Docker and Containerization]]
- [[Jenkins (CI and CD)]]

## Sources
- DevOps Engineering Exam Preparation.md
