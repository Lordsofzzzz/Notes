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
The technology and workflow for packaging software into standardized units for development, shipment, and deployment. Containerization ensures that applications run consistently across different environments by bundling the code with its dependencies, libraries, and configuration.

### Containerization Workflow
1. **Build**: Create a Docker image from a `Dockerfile`.
2. **Ship**: Tag and push the image to a central registry.
3. **Run**: Deploy and execute the container on any host system.

### Flask App Example
```bash
docker build -t flask-app .
docker run -d -p 5000:5000 --name web flask-app
docker stop web && docker rm web
```

### Common Docker Commands
- **`docker build -t myapp:latest .`**: Build an image from the current directory's `Dockerfile`.
- **`docker tag myapp:latest myuser/myapp:v1`**: Tag an image for a specific user and version to prepare for pushing.
- **`docker push myuser/myapp:v1`**: Push a tagged image to a remote registry (e.g., Docker Hub).
- **`docker run -d -p 8080:80 myapp`**: Run a container in the background (`-d`) with host port 8080 mapped to container port 80.
- **`docker stop <container_name>`**: Gracefully stop a running container.
- **`docker rm <container_name>`**: Remove a stopped container from the host.

### Docker Architecture
The Docker system operates on a client-server architecture:
- **Docker Client**: The CLI tool used by developers to issue commands.
- **Docker Daemon (dockerd)**: The background process that listens for API requests and manages Docker objects (images, containers, networks, volumes).
- **Docker Host**: The physical or virtual machine where the daemon runs.
- **Docker Registry**: Central storage for images. Public (Docker Hub) or private (Azure Container Registry, ECR).

### Docker Compose
Docker Compose defines and manages multi-container applications using a `docker-compose.yml` file. It allows you to start an entire stack (e.g., Web App + Database) with a single command.
```yaml
version: '3.8'
services:
  web:
    image: my-web-app:latest
    ports:
      - "80:80"
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: secretpassword
```

### Key Concepts
- **Image**: A read-only template (the "blueprint") for creating containers.
- **Container**: A stateless, runnable instance of an image.
- **Layers**: Each instruction in a `Dockerfile` (e.g., `FROM`, `RUN`, `COPY`) creates a new read-only layer. Layers are cached to speed up subsequent builds.

## Associative Trails
Containerization solves the "it works on my machine" problem in the DevOps workflow. This note exists to document the standard build-ship-run cycle and refines the transition from traditional VMs to lightweight, portable application units.

## Connections
- [[DevOps Engineering (MOC)]]
- [[Kubernetes (K8s) Architecture]]
- [[Jenkins (CI and CD)]]

## Sources
- DevOps Engineering Exam Preparation.md
