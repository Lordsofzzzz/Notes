---
status: seedling
tags: [devops, moc, principles]
created: 2026-04-10
updated: 2026-04-10
---

# DevOps Engineering (MOC)

## Summary
A Map of Content (MOC) for the principles, methodologies, and tools that integrate software development and operations.

## Details
Set of practices, tools, and a cultural philosophy that automates and integrates processes between software development and IT operations teams.

### Core Principles (CAMS)
- **C**ulture: Removing silos and promoting shared responsibility.
- **A**utomation: Replacing manual tasks (testing, deployment) with automated workflows.
- **M**easurement: Monitoring and logging for performance and health.
- **S**haring: Promoting open communication and knowledge sharing.

### CI/CD Pipeline
- **Source**: Code commit triggers the pipeline.
- **Build**: Compiles code and creates artifacts (e.g., Maven, Docker images).
- **Test**: Automated unit, integration, and UI tests (e.g., Selenium).
- **Deploy**: Pushing artifacts to staging or production.

### Technology Stack
- **SCM**: [[Git (Version Control)]] (Workflows, branches, conflict resolution).
- **CI/CD**: [[Jenkins (CI and CD)]] (Declarative pipelines, webhooks).
- **Containerization**: [[Docker and Containerization]] (Build, Tag, Push, Compose).
- **Orchestration**: [[Kubernetes (K8s) Architecture]] (Deployments, Services, Autoscaling).
- **Monitoring**: [[Prometheus and Nagios (Monitoring)]] (Pull-based vs Agent-based).
- **Logging**: [[ELK Stack (Logging)]] (Beats, Logstash, Elasticsearch, Kibana).
- **Visualization**: [[Grafana (Visualization)]] (PromQL dashboards).

## Associative Trails
This Map of Content serves as the top-level index for all DevOps notes in the vault. It refines the [[Linux Systems (MOC)]] by adding higher-level automation and collaboration concepts, challenging the traditional siloed approach to software delivery.

## Connections
- [[Linux Systems (MOC)]]
- [[Kubernetes (K8s) Architecture]]
- [[Docker and Containerization]]
- [[Jenkins (CI and CD)]]
- [[Git (Version Control)]]
- [[ELK Stack (Logging)]]
- [[Prometheus and Nagios (Monitoring)]]

## Sources
- DevOps Engineering Exam Preparation.md
