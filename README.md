# SRE Observability Stack

> Full-stack observability platform for production Kubernetes environments.
> Implements the four golden signals with automated SLO tracking and intelligent alerting.

---

## Stack Components

| Layer | Tool | Purpose |
|---|---|---|
| Metrics | Prometheus + Thanos | Long-term metric storage and global view |
| Visualization | Grafana | Dashboards, SLO tracking |
| Logs | Loki + Promtail | Structured log aggregation |
| Traces | Tempo + OpenTelemetry | Distributed tracing |
| Alerting | Alertmanager + PagerDuty | Intelligent alert routing |
| Uptime | Blackbox Exporter | Synthetic monitoring |

## SLO Framework

`
Service: Payment API
|- Availability SLO:  99.95% (error budget: 21.9 min/month)
|- Latency SLO:       p99 < 500ms for 99.5% of requests
+- Throughput SLO:    Handle 10,000 RPS without degradation

Error Budget Policy:
  100% - 75% remaining  -> Normal operations
  75%  - 25% remaining  -> Increased deployment caution
  25%  - 0%  remaining  -> Feature freeze, reliability work only
  0%   remaining        -> Incident review required before deploys
`

## Alert Severity Levels

| Severity | Response Time | Examples |
|---|---|---|
| P1 - Critical | 5 min (PagerDuty call) | Service down, error rate >5% |
| P2 - High | 15 min (PagerDuty SMS) | p99 latency >2s, pod crashlooping |
| P3 - Medium | 4 hours (Slack) | Memory >85%, disk >80% |
| P4 - Low | Next business day | Non-critical deprecated API usage |

## Deploy

`ash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
kubectl apply -k observability/
helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f values/prometheus-values.yaml
`

---
Senior DevOps Engineer / SRE - Vikas Rao