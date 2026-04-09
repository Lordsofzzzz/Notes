---
status: seedling
tags: [devops, docker, containerization]
created: 2026-04-10
updated: 2026-04-10
---

# Docker and Containerization

## Summary
The technology and workflow for packaging software into standardized units called containers.

## Details
Packaging of software into standardized units for development, shipment, and deployment.

### Containerization Workflow
1. **Build**: Create a Docker image from a `Dockerfile`.
2. **Ship**: Tag and push the image to a registry.
3. **Run**: Deploy and execute the container on any host.

### Common Docker Commands
- **`docker build -t myapp:latest .`**: Build an image from the current directory.
- **`docker tag myapp:latest myuser/myapp:v1`**: Tag an image for a specific user and version.
- **`docker push myuser/myapp:v1`**: Push an image to a remote registry.
- **`docker run -d -p 8080:80 myapp`**: Run a container in the background with port mapping.
- **`docker stop <container_name>`**: Stop a running container.
- **`docker rm <container_name>`**: Remove a stopped container.

### Architecture
- **Docker Client**: The CLI interface.
- **Docker Daemon**: The background process that manages images and containers.
- **Docker Host**: The server where containers are running.
- **Docker Registry**: The central storage for Docker images (e.g., Docker Hub).

### Docker Compose
Docker Compose is used for defining and running multi-container applications (e.g., a web service and its database) using a `docker-compose.yml` file.

### Comparison of Concepts
- **Image**: A read-only template with instructions for creating a Docker container.
- **Container**: A runnable instance of an image.
- **Layers**: Each instruction in a `Dockerfile` creates a layer in the image.

## Associative Trails
Containerization solves the "it works on my machine" problem in the DevOps workflow. This note exists to document the standard build-ship-run cycle and refines the transition from traditional VMs to lightweight, portable application units.

## Connections
- [[DevOps Engineering (MOC)]]
- [[Kubernetes (K8s) Architecture]]
- [[Jenkins (CI and CD)]]

## Sources
- DevOps Engineering Exam Preparation.md
