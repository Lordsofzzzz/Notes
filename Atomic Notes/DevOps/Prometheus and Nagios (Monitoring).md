---
status: seedling
tags: [devops, monitoring, alerting]
created: 2026-04-10
updated: 2026-04-10
---

# Prometheus and Nagios (Monitoring)

## Summary
Core infrastructure monitoring systems used for pull-based metrics (Prometheus) and agent-based status checks (Nagios).

## Details
Monitoring and alerting are the "eyes and ears" of a production environment, ensuring that engineers are notified of issues before they become outages.

### Comparison of Monitoring Models
- **Prometheus (Pull-based)**:
  - **Pull Model**: Scrapes metrics from targets (exporters) at regular intervals.
  - **Data Model**: Uses a multi-dimensional time-series data model (metric names + key-value labels).
  - **Alerting**: Uses PromQL to define alert rules (e.g., `node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes * 100 < 10`).
  - **Best For**: Scalable, containerized environments (Kubernetes).
- **Nagios (Agent-based)**:
  - **Agent-based**: Uses specialized agents (e.g., NRPE) installed on each host to report health status.
  - **Checks**: Executable commands for status checks.
    - Example CPU Check: `check_command check_nrpe!check_cpu!85!90`.
    - 85% = Warning, 90% = Critical.
  - **Best For**: Traditional VM-based infrastructure and host-specific health checks.

### Alert Configuration Thresholds
- **Warning Threshold**: The level at which an alert is triggered, indicating attention is needed (e.g., 85% CPU).
- **Critical Threshold**: The level at which immediate action is required (e.g., 90% CPU).
- **Measurement Logic**: In Prometheus, we calculate the percentage of memory available vs total.

## Associative Trails
Monitoring is the eyes and ears of any production environment. This note documents the two primary models of infrastructure monitoring (pull-based vs. agent-based), refining simple status checks into a scalable observability framework.

## Connections
- [[DevOps Engineering (MOC)]]
- [[Grafana (Visualization)]]
- [[ELK Stack (Logging)]]

## Sources
- DevOps Engineering Exam Preparation.md
