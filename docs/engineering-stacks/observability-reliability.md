# 📈 Universal SaaS: Observability & Reliability Stack

This document defines the **Nerve System** of the platform—synthesizing **OpenTelemetry (OTel)**, **ClickHouse (The ClickStack)**, and **SRE (Site Reliability Engineering)** golden signals. It is the blueprint for industrial-grade visibility across the 9-tier microservices stack.

---

## 🏛️ 01. The ClickStack Architecture (Logs, Metrics, Traces)

We move away from fragmented monitoring tools into a unified, high-performance telemetry backbone.

- **Unified Backend: ClickHouse**: Every log line, metric point, and distributed trace is stored in specialized, compressed columnar tables in ClickHouse.
- **The Gateway: OTel Collector**: Every container (Go, Nginx, Redis) exports telemetry to a central **OpenTelemetry Collector** sidecar, which batches and sinks data into ClickHouse.

---

## 📊 02. The 4 Golden Signals (Real-time SRE)

We monitor the platform's health through the lens of the **Google SRE Model**.

1.  **Latency**: Time to service a request (p50, p95, p99). Target: Sub-100ms for core API handlers.
2.  **Traffic**: Demand placed on the 9 tiers (Requests per second, Message queue depth).
3.  **Errors**: Rate of requests that fail (Explicitly mapped to the `error_taxonomy.md`).
4.  **Saturation**: How "full" the service is (CPU/RAM limits, Connection pool exhaustion).

---

## 🍞 03. Distributed Tracing (The Breadcrumb Trail)

In a 9-tier architecture, a single WhatsApp message triggers a chain of events. We trace this journey in < 1ms resolution.

- **Trace Context Propagation**: Every request carries a `trace_parent` header across the network (Nginx -> Go App -> Worker -> Redis).
- **Industrial Visualization**: Using **Grafana** or **Jaeger** to visualize the lifecycle of a transaction, pinpointing which service in the 9-tier stack caused a latency spike.

---

## 🔔 04. Alerting & SLI/SLO Governance

We prioritize "Symptoms over Causes."

- **Service Level Indicators (SLI)**: Specific metrics we measure (e.g., WhatsApp delivery success rate).
- **Service Level Objectives (SLO)**: The targets we MUST hit (e.g., 99.9% delivery success).
- **Error Budget**: If errors exceed the SLO, feature development is paused to focus on reliability (Zerodha/Zoho-style engineering discipline).

---

## 🛡️ 05. Reliability Patterns (Circuit Breakers & Retries)

We design for failure.

- **Circuit Breaker (Self-Healing)**: If an upstream service (like the Meta API) is failing, the platform "trips" the circuit and returns an `ERR_UPSTREAM_DOWN` instantly, preventing a cascade of stalled Goroutines.
- **Exponential Backoff Retries**: Failed logic (e.g., pushing a message to a queue) is retried with increasing delays to allow the system to recover from transient spikes.

---

## 🚀 06. Container-Native Instrumentation

Every Docker container must follow the **Industrial Observability Contract**:

- **Healthchecks**: Mandatory `HEALTHCHECK` instructions in every `Dockerfile` for Kubernetes-native liveness/readiness detection.
- **Logs-to-StdOut**: All containers log in JSON format to `stdout`. The OTel collector/FluentBit scrapes these logs without disk I/O overhead.
- **Resource Profiling**: Real-time **pprof** (Go) endpoints exposed on a private `/debug` port for on-the-fly bottleneck analysis.

## 🚀 07. Specialized Observability & Debugging (Sovereign Tools)

To enhance visibility into specific communication rails and simplify local development, the following high-agency tools are utilized:

- **Email Observability:** **[Sessy](https://github.com/marckohlbrugge/sessy)** — Open-source email observability specifically for AWS SES. Provides deep insights into delivery rates, bounces, and complaints within the sovereign infrastructure.
- **Enhanced Debugging:** **[Slogx](https://github.com/binhonglee/slogx)** — "Good ol' print debugging, but better." A TypeScript-native tool for structured, readable, and efficient local debugging without the clutter of standard console logs.

---
*Last Updated: April 2026 | Following the Universal SaaS Engineering Standard — BlackLoverTech*
