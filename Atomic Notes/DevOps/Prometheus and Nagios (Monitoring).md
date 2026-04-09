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
Systems for monitoring and alerting in IT infrastructure.

### Comparison of Systems
- **Prometheus**: A modern, pull-based monitoring system.
  - **Pull Model**: Scrapes metrics from targets at regular intervals.
  - **Alerting**: Uses `node_cpu_seconds_total` and PromQL for defining thresholds.
  - **Visualization**: Best used with [[Grafana]].
- **Nagios**: An older, agent-based monitoring system.
  - **Agent-based**: Uses specialized agents (e.g., NRPE) to report status.
  - **Checks**: Executable commands for status (e.g., `check_nrpe!check_cpu!85!90`).

### Alert Configuration
- **Warning Threshold**: The level at which an alert is triggered (e.g., 85% CPU).
- **Critical Threshold**: The level at which immediate action is required (e.g., 90% CPU).

## Associative Trails
Monitoring is the eyes and ears of any production environment. This note documents the two primary models of infrastructure monitoring (pull-based vs. agent-based), refining simple status checks into a scalable observability framework.

## Connections
- [[DevOps Engineering (MOC)]]
- [[Grafana (Visualization)]]
- [[ELK Stack (Logging)]]

## Sources
- [[DevOps Engineering Exam Preparation.md]]
