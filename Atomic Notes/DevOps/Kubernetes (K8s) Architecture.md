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
Open-source system for automating deployment, scaling, and management of containerized applications.

### Cluster Components
- **Control Plane**: Manages the cluster.
  - **API Server**: The interface for interacting with the cluster.
  - **Scheduler**: Allocates pods to nodes.
  - **etcd**: The central storage for cluster state.
- **Worker Nodes**: Run applications.
  - **Kubelet**: An agent that ensures pods are running on its node.
  - **Kube-proxy**: Manages networking for pods.
  - **Pods**: The smallest unit of deployment in K8s (one or more containers).

### Service Types
- **ClusterIP**: Exposes the service on a cluster-internal IP (default).
- **NodePort**: Exposes the service on each node's IP at a static port.
- **LoadBalancer**: Exposes the service externally using a cloud provider's load balancer.

### Deployment Features
- **Rolling Updates**: Zero-downtime updates by incrementally replacing pods.
- **Rollbacks**: Reverting a deployment to a previous version (`kubectl rollout undo`).
- **Horizontal Pod Autoscaling (HPA)**: Scaling the number of pods based on CPU/Memory thresholds.

### Deployment Manifest Example
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
```

### Helm
Helm is the package manager for Kubernetes.
- **Chart.yaml**: Metadata for the chart.
- **values.yaml**: Configurable parameters.
- **templates/**: YAML manifests for K8s resources.

## Associative Trails
K8s is the industry standard for managing containerized applications at scale. This note exists to document the core architecture and resource models, refining the transition from single-host [[Docker and Containerization]] to cluster-wide orchestration.

## Connections
- [[DevOps Engineering (MOC)]]
- [[Docker and Containerization]]
- [[Jenkins (CI and CD)]]

## Sources
- DevOps Engineering Exam Preparation.md
