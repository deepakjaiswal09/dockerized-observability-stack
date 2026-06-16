# 🚀 Standalone Observability & Monitoring Stack

## Overview

This project is a complete, production-ready observability platform built using industry-standard open-source tools. It provides centralized monitoring, logging, distributed tracing, infrastructure visibility, and alerting for any application or microservice architecture.

The stack is designed to run independently and can be integrated with any service by connecting it to the shared Docker network and exposing metrics, logs, and traces.

---

# Architecture

```text
Applications / Microservices
            │
            ▼

 ┌───────────────────────────┐
 │     Observability Stack   │
 └───────────────────────────┘

      ┌───────────────┐
      │ Prometheus    │ ◄── Metrics
      └──────┬────────┘
             │
             ▼
      ┌───────────────┐
      │ Grafana       │
      └───────────────┘

      ┌───────────────┐
      │ Fluent Bit    │ ◄── Container Logs
      └──────┬────────┘
             ▼
      ┌───────────────┐
      │ Elasticsearch │
      └──────┬────────┘
             ▼
      ┌───────────────┐
      │ Kibana        │
      └───────────────┘

      ┌───────────────┐
      │ OpenTelemetry │ ◄── Traces
      └──────┬────────┘
             ▼
      ┌───────────────┐
      │ Jaeger        │
      └───────────────┘

      ┌───────────────┐
      │ Alertmanager  │
      └───────────────┘

      ┌───────────────┐
      │ Node Exporter │
      └───────────────┘

      ┌───────────────┐
      │ cAdvisor      │
      └───────────────┘
```

---

# Components

## Grafana

Visualization and dashboarding platform.

Features:

* Infrastructure dashboards
* Application dashboards
* Alerting
* Custom visualizations

Access:

```bash
http://localhost:3000
```

---

## Prometheus

Metrics collection and storage.

Collects:

* Host metrics
* Container metrics
* Application metrics
* Custom business metrics

Access:

```bash
http://localhost:9090
```

---

## Alertmanager

Handles Prometheus alerts.

Supports:

* Email
* Slack
* Teams
* Webhooks

Access:

```bash
http://localhost:9093
```

---

## Fluent Bit

Lightweight log collector.

Collects:

* Docker container logs
* Application logs
* System logs

Forwards logs to Elasticsearch.

---

## Elasticsearch

Stores logs for indexing and searching.

Access:

```bash
http://localhost:9200
```

---

## Kibana

Log exploration and visualization.

Features:

* Log search
* Filtering
* Correlation
* Error investigation

Access:

```bash
http://localhost:5601
```

---

## Jaeger

Distributed tracing platform.

Features:

* Request tracing
* Service dependency mapping
* Latency analysis
* Root cause analysis

Access:

```bash
http://localhost:16686
```

---

## Node Exporter

Collects host-level metrics.

Metrics include:

* CPU
* Memory
* Disk
* Network

---

## cAdvisor

Collects container-level metrics.

Metrics include:

* Container CPU
* Container Memory
* Filesystem usage
* Network usage

Access:

```bash
http://localhost:8080
```

---

# Features

## Metrics Monitoring

Monitors:

* CPU utilization
* Memory consumption
* Disk usage
* Network activity
* Container health
* Application performance

Powered by:

* Prometheus
* Node Exporter
* cAdvisor

---

## Centralized Logging

Provides:

* Container log aggregation
* Log search
* Error analysis
* Historical log retention

Powered by:

* Fluent Bit
* Elasticsearch
* Kibana

---

## Distributed Tracing

Provides:

* End-to-end request visibility
* Latency breakdown
* Error tracing
* Service dependency analysis

Powered by:

* OpenTelemetry
* Jaeger

---

## Alerting

Supports:

* CPU alerts
* Memory alerts
* Service down alerts
* Application health alerts
* Custom business alerts

Powered by:

* Prometheus
* Alertmanager
* Grafana

---

# Project Structure

```text
observability-stack/

├── prometheus/
│   ├── prometheus.yml
│   └── alert.rules.yml
│
├── alertmanager/
│   └── alertmanager.yml
│
├── elasticsearch/
│   └── elasticsearch.yml
│
├── kibana/
│   └── kibana.yml
│
├── fluentbit/
│   └── fluent-bit.conf
│
├── grafana/
│   └── provisioning/
│       ├── dashboards/
│       └── datasources/
│
├── jaeger/
│
├── node-exporter/
│
├── cadvisor/
│
├── docker-compose.yml
│
└── .env
```

---

# Running the Stack

Start all services:

```bash
docker compose up -d
```

Verify containers:

```bash
docker ps
```

---

# Integrating a New Service

## Step 1: Connect to Shared Network

In your service compose file:

```yaml
networks:
  observability-network:
    external: true
```

Attach service:

```yaml
services:
  my-service:
    networks:
      - observability-network
```

---

## Step 2: Expose Metrics

Install:

```bash
npm install prom-client
```

Example:

```typescript
import client from "prom-client";

client.collectDefaultMetrics();

app.get("/metrics", async (_, res) => {
  res.set("Content-Type", client.register.contentType);
  res.end(await client.register.metrics());
});
```

---

## Step 3: Configure Prometheus

Add scrape target:

```yaml
- job_name: my-service
  static_configs:
    - targets:
        - my-service:3000
```

Restart Prometheus:

```bash
docker compose restart prometheus
```

---

## Step 4: Enable Distributed Tracing

Install:

```bash
npm install \
@opentelemetry/sdk-node \
@opentelemetry/auto-instrumentations-node \
@opentelemetry/exporter-trace-otlp-http
```

Example:

```typescript
new OTLPTraceExporter({
  url: "http://jaeger:4318/v1/traces"
});
```

Service will automatically appear inside Jaeger.

---

## Step 5: Enable Logging

No application changes required.

Fluent Bit automatically collects Docker logs.

Recommended:

Use structured JSON logging.

Example:

```json
{
  "level": "info",
  "message": "Notification sent",
  "userId": "123"
}
```

---

# Verification

## Prometheus

Open:

```bash
http://localhost:9090
```

Check:

```promql
up
```

---

## Metrics Endpoint

```bash
http://localhost:<PORT>/metrics
```

Expected:

```text
# HELP ...
# TYPE ...
```

---

## Kibana

Open:

```bash
http://localhost:5601
```

Search:

```text
container_name
```

Verify logs are flowing.

---

## Jaeger

Open:

```bash
http://localhost:16686
```

Verify:

* Service appears
* Traces are generated
* Operations are visible

---

# Use Cases

Suitable for:

* Node.js Applications
* Java Applications
* Python Applications
* Microservices
* Monoliths
* Event-Driven Systems
* Containerized Workloads
* Kubernetes Migration

---

# Benefits

* Centralized Monitoring
* Centralized Logging
* Distributed Tracing
* Infrastructure Visibility
* Faster Incident Resolution
* Root Cause Analysis
* Performance Optimization
* Production Readiness

---

# Author

Deepak Jaiswal

DevOps Engineer | Platform Engineering | Observability Engineering

Built as a reusable standalone observability platform capable of monitoring, tracing, logging, and alerting for any modern application stack.
