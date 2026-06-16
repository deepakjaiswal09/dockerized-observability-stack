🚀 Standalone Observability & Monitoring Stack

A production-ready, containerized observability platform designed to provide Metrics, Logs, Traces, Alerting, and Infrastructure Monitoring for any application or microservice architecture.

This stack can run completely standalone or be integrated with existing applications with minimal configuration.

📌 Overview

Modern distributed systems require visibility into application health, infrastructure performance, errors, logs, and request flows.

This project provides a complete observability solution using industry-standard open-source tools:

Component	Purpose
Grafana	Visualization & Dashboards
Prometheus	Metrics Collection
Alertmanager	Alert Routing & Notifications
Fluent Bit	Log Collection & Forwarding
Elasticsearch	Log Storage & Search
Kibana	Log Analytics
Jaeger	Distributed Tracing
OpenTelemetry	Trace Generation
Node Exporter	Host Metrics
cAdvisor	Container Metrics
Docker Compose	Orchestration
🏗 Architecture
┌─────────────────────────────┐
│      Application(s)         │
│ NodeJS / Java / Python      │
└────────────┬────────────────┘
             │
 ┌───────────┼───────────┐
 │           │           │
 ▼           ▼           ▼

Metrics     Logs      Traces
(Prometheus)(FluentBit)(OTEL)

 │           │           │
 ▼           ▼           ▼

Prometheus Elasticsearch Jaeger

 │           │
 ▼           ▼

Grafana     Kibana

 │
 ▼

Alertmanager
 │
 ▼
Email / Slack / Teams
✨ Features
📊 Metrics Monitoring

Collects:

CPU Usage
Memory Usage
Disk Usage
Network Traffic
Container Metrics
Application Metrics
Custom Business Metrics

Powered by:

Prometheus
Node Exporter
cAdvisor
📜 Centralized Logging

Collects logs from:

Docker Containers
Application Logs
Backend Services
Workers

Features:

Full-text Search
Log Filtering
Log Correlation
Error Investigation

Powered by:

Fluent Bit
Elasticsearch
Kibana
🔍 Distributed Tracing

Track requests across services.

Provides:

Request Flow Visualization
Latency Analysis
Error Tracking
Bottleneck Detection

Powered by:

OpenTelemetry
Jaeger
🚨 Alerting

Supports:

Email Alerts
Slack Alerts
Microsoft Teams
Webhooks

Examples:

High CPU Usage
Memory Threshold Breach
Container Down
Application Errors
Service Unavailability

Powered by:

Prometheus Alert Rules
Alertmanager
Grafana Alerting
🖥 Infrastructure Monitoring

Monitor:

Docker Containers
Host System
CPU
RAM
Disk
Network
Container Resource Consumption

Powered by:

Node Exporter
cAdvisor
📁 Project Structure
observability-stack/
│
├── prometheus/
│   ├── prometheus.yml
│   └── alert.rules.yml
│
├── alertmanager/
│   └── alertmanager.yml
│
├── fluentbit/
│   └── fluent-bit.conf
│
├── grafana/
│   └── provisioning/
│       ├── datasources/
│       └── dashboards/
│
├── elasticsearch/
│   └── elasticsearch.yml
│
├── kibana/
│   └── kibana.yml
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
🐳 Included Services
Grafana

Access:

http://localhost:3000

Purpose:

Dashboards
Alerting
Metrics Visualization
Prometheus

Access:

http://localhost:9090

Purpose:

Metrics Storage
Alert Rule Evaluation
Service Scraping
Alertmanager

Access:

http://localhost:9093

Purpose:

Alert Routing
Email Notifications
Alert Grouping
Elasticsearch

Access:

http://localhost:9200

Purpose:

Log Storage
Log Search
Kibana

Access:

http://localhost:5601

Purpose:

Log Visualization
Log Analytics
Jaeger

Access:

http://localhost:16686

Purpose:

Distributed Tracing
Trace Analysis
Node Exporter

Access:

http://localhost:9100/metrics

Purpose:

Host Metrics
cAdvisor

Access:

http://localhost:8080

Purpose:

Container Metrics
🔌 Integrating Any Service

Any service can integrate with this stack.

Metrics Integration

Expose:

/metrics

using:

NodeJS
npm install prom-client

Example:

import client from "prom-client";

client.collectDefaultMetrics();

app.get("/metrics", async (_, res) => {
  res.set("Content-Type", client.register.contentType);
  res.end(await client.register.metrics());
});

Then add to:

prometheus.yml
- job_name: my-service
  static_configs:
    - targets:
        - my-service:3000
Logging Integration

Docker logs are automatically collected by Fluent Bit.

No application changes required.

Optional:

Use structured JSON logs.

Example:

{
  "level": "info",
  "message": "User login",
  "userId": "123"
}
Tracing Integration

Install OpenTelemetry.

Example:

npm install \
@opentelemetry/sdk-node \
@opentelemetry/auto-instrumentations-node

Configure:

new OTLPTraceExporter({
  url: "http://jaeger:4318/v1/traces"
});

Service appears automatically in Jaeger.

📧 Alerting Setup

Configure:

alertmanager.yml

Example:

receivers:
  - name: email
    email_configs:
      - to: alerts@example.com

Supports:

Gmail
Outlook
SMTP Servers
Slack
Teams
Webhooks
📈 Prebuilt Dashboards

Included dashboards:

Node Exporter Full

Provides:

CPU Usage
Memory Usage
Disk Usage
Network Usage
Docker Monitoring

Provides:

Container CPU
Container Memory
Container Restarts
Container Health
Application Metrics

Provides:

Request Count
Response Time
Error Rate
Active Connections
🔒 Security Recommendations

Production deployments should:

Use TLS
Enable Grafana Authentication
Secure Elasticsearch
Restrict Dashboard Access
Store Secrets in Vault
Use Docker Secrets
🚀 Startup

Run:

docker compose up -d

Verify:

docker ps
🧪 Health Checks

Grafana:

http://localhost:3000

Prometheus:

http://localhost:9090

Kibana:

http://localhost:5601

Jaeger:

http://localhost:16686
🎯 Use Cases

Suitable for:

Microservices
Monolith Applications
SaaS Platforms
E-Commerce Systems
Backend APIs
Event-Driven Architectures
Containerized Workloads
Kubernetes Migration Readiness
🏆 Outcomes

This stack enables:

✅ Centralized Monitoring
✅ Centralized Logging
✅ Distributed Tracing
✅ Infrastructure Monitoring
✅ Application Monitoring
✅ Proactive Alerting
✅ Faster Incident Resolution
✅ Root Cause Analysis
✅ Performance Optimization
✅ Production Observability

Author

Deepak Jai Shopcardd
DevOps | Platform Engineering | Observability Engineering

Built as a reusable standalone observability platform capable of monitoring and troubleshooting any modern application stack.
