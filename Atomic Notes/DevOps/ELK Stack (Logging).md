---
status: seedling
tags: [devops, logging, elk]
created: 2026-04-10
updated: 2026-04-10
---

# ELK Stack (Logging)

## Summary
A distributed logging system composed of Elasticsearch, Logstash, Kibana, and Beats for log management and visualization.

## Details
Distributed logging system used for collecting, parsing, storing, and visualizing logs.

### Components
- **Beats**: Light-weight agents that collect and forward logs (e.g., Filebeat).
- **Logstash**: A server-side data processing pipeline that parses and filters logs.
- **Elasticsearch**: A search and analytics engine that stores and indexes logs.
- **Kibana**: A visualization layer for creating dashboards and searching logs.

### Workflow
```
Beats → Logstash → Elasticsearch → Kibana (Dashboard)
```

### Importance in DevOps
- **Centralization**: Aggregates logs from multiple distributed services.
- **Traceability**: Allows tracing a request across a microservices architecture.
- **Troubleshooting**: Helps identify issues and patterns in production.

## Associative Trails
Essential for understanding system health and debugging distributed applications. This note documents the standard pipeline for centralizing distributed logs, refining the manual process of searching files across multiple servers.

## Connections
- [[DevOps Engineering (MOC)]]
- [[Prometheus and Nagios (Monitoring)]]
- [[Grafana (Visualization)]]

## Sources
- DevOps Engineering Exam Preparation.md
