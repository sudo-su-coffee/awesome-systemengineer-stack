<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:4facfe,100:00f2fe&height=200&section=header&text=Infrastructure%20Stack&fontSize=60&fontAlignY=35&desc=Industrial%20SaaS%20Engineering%20Blueprint" width="100%" alt="Infrastructure Stack Banner"/>
</div>

# 🏛️ Universal Infrastructure Engineering Stack

This document defines the "Zerodha-Grade" high-performance infrastructure for a modern, multi-tenant SaaS platform. This design emphasizes absolute service decoupling, ultra-minimalist runtimes, and a "Zero-Trust" internal security model.

## 1. The 7-Service Architectural Suite

The platform is decomposed into specialized, horizontally-scalable units.

| Service | Identity | Role | Runtime |
|---|---|---|---|
| **`gateway`** | Ingress | The Traffic Conductor (Nginx). The **only** service exposed to the public internet. | `nginx:alpine` |
| **`frontend`** | Interface | Isolated UI container. Serves static React/Vue/Svelte assets. | `nginx:alpine` |
| **`media`** | CDN Origin | The "Simple CDN" service for `./uploads` and `./audio`. | `nginx:alpine` |
| **`app`** | Core API | The high-performance, stateless Go logic core. | `alpine` |
| **`worker`** | Worker | Isolated compute engine for heavy/async processing. | `alpine/debian-slim` |
| **`db`** | Storage | Hardened PostgreSQL instance (The source of truth). | `postgres:alpine` |
| **`redis`** | Coordination | Redis-backed Pub/Sub, Rate-limiting, and distributed locking. | `redis:alpine` |
| **`observe`** | Visibility | The OTel (OpenTelemetry) + ClickHouse stack for industrial-grade logging. | `otel/clickhouse` |
| **`vault`** | Secrets | Hardware/Software security module for secrets injection. | `vault:alpine` |

---

## 2. 💎 The Economic Engine Cluster (Isolated Suite)

To ensure maximum financial security and absolute ledger integrity, the **Economic Engine** resides in a dedicated, isolated cluster. This 3-layer suite is logically decoupled from the main application API.

| Layer | Service | Role | Industrial Image |
|---|---|---|---|
| **01. Orchestra** | `hyperswitch` | The Payments Smart-Router (Managing R-Pay, Stripe, Paytm). | `juspay/hyperswitch-router` |
| **02. Log & Meter** | `lago` | Real-time metering, financial memory, and dunning kernel. | `getlago/lago-api` |
| **03. Visibility** | `viz-dashboard` | Financial observability, reconciliation, and audit dashboards. | `metabase:latest` |

---

## 2. Security & Zero-Trust Topology

### Ingress Filtering & Edge WAF
- **Hardened Shell**: Only the `gateway` (Nginx) listens on ports 80/443. All other services reside on an isolated internal network (`The Platform-internal`).
- **Edge Layer Protection**: The gateway is configured with **ModSecurity/Naxsi WAF** rules and aggressive rate-limiting to prevent DDoS and credential stuffing.

### Secrets & Identity Hygiene (Zero-Trust)
- **No Env Secrets**: Production secrets MUST NOT be stored in `.env`. Use **Secret Injection** patterns (Tier 9) where the `vault` container injects read-only files at `/run/secrets/`.
- **Identity-Based Auth**: Internal service calls (e.g., App to TTS) utilize mTLS or internal API tokens to ensure that even a compromised container cannot spoof internal requests.

---

## 3. CDN & Media Strategy (The Zoho Model)

### Asset Delivery
- **Direct Fetch**: The `app` returns absolute CDN URLs (e.g., `https://cdn.example.com/audio/v1.ogg`). This bypasses the application server entirely, saving CPU and bandwidth.
- **Origin Isolation**: The `media` container acts as the stable origin for the CDN. It provides high-speed file delivery with aggressive `Cache-Control` headers.

### Pre-Signed Logic
- For sensitive uploads, the `app` generates pre-signed expiry URLs. The user device fetches directly from the `media` origin or CDN, ensuring the main API is never a bottleneck for binary data transfers.

---

## 4. Zerodha-Style Scaling & Performance

### Vertical Isolation (Tenant Sandboxing)
- **PostgreSQL RLS**: Every table utilizes **Row-Level Security** keyed to `org_id`. The DB user used by the `app` cannot physically query data belonging to a different tenant, even in the event of a SQL injection vulnerability (Zoho-level multi-tenancy).
- **ClickHouse Sharding**: Analytical data is sharded by `account_id` to ensure that one tenant's heavy dashboard queries never impact the real-time message delivery of another (Industrial QoS).

### Minimalist Runtimes
- **Alpine Primacy**: Every service (where possible) uses the ~5MB Alpine Linux base to minimize the attack surface and maximize container density.
- **Decoupled TTS**: Resource-heavy Piper processes are isolated to the `tts` worker, ensuring IVR generation never starves the main CRM API of CPU cycles.

### Redis Coordination
- Redis is utilized as a **State Registry** (Zoho-style). It tracks real-time agent presence and manages the atomic locking required for multi-tenant webhook processing.

---

## 5. Deployment Guidelines

### Containerized (Docker Compose)
Use the provided `docker-compose.yml` to orchestrate all 7 services with built-in healthchecks and resource cgroup limits.

### Bare Metal Migration
To achieve "Mechanical Sympathy" on raw hardware:
### Secrets Management (Tier 9: `vault`)
A dedicated layer for managing sensitive environment variables and `config.toml` values.
- **Protocol**: The `vault` service (or Docker Secrets subsystem) injects read-only files into the `app` container at bootstrap. 
- **Benefit**: No secrets are ever stored in the container image or the process environment (`env`), preventing leakage via debug dumps or logs.

---

## 6. Engineering Lifecycle Protocols

### Observability: The OTel + ClickHouse Stack
Why ClickHouse over Prometheus?
- **Prometheus**: Optimized for **metrics** (CPU, RAM, 1-minute averages).
- **ClickHouse**: Optimized for **logs and traces** (The Platform generates millions of webhook events per day).
- **Architecture**: The `app` exports OTLP (OpenTelemetry Protocol) to the `observe` collector, which then batches and sinks records into ClickHouse for historical analysis.

### Go Instrumentation: OTel Pattern
To instrument the Go backend at a "Zerodha Level":
1.  **Transport**: Use `go.opentelemetry.io/otel/exporters/otlp/otlptrace/otlptracegrpc`.
2.  **Traces**: Use `otel.Tracer` to wrap HTTP handlers. Every `ERR_` code is attached to the span as an attribute (`err.code`).
3.  **Logs**: Zap/Logf entries are enriched with `trace_id`, making log-to-trace correlation instant in Grafana.

### Schema Migration (Sidecar Pattern)
To ensure database integrity during "Blue/Green" deployments, use a dedicated **Migration Sidecar**. The sidecar runs `golang-migrate` before the `app` starts, ensuring the schema is locked and updated before the first API request hits the DB.

### Code-to-Doc Parity (CI Enforcement)
The `standard_error_codes.md` is an enforceable contract.
- **Protocol**: A CI linter scans the codebase for `SendErrorEnvelope` calls.
- **Rule**: Any error code used in Go that is NOT documented in the Markdown file results in a failed CI build.

### Governance: Data Sovereignty
To meet DPDP/GDPR standards, the architecture supports **Geographic Pinning**.
- Multi-region clusters can be deployed where `db` and `media` are region-locked while the `gateway` handles global routing.

### Architecture Principle: User Disengagement (Zerodha Style)
Our engineering philosophy prioritizes **Deterministic Reliability**. We don't want users "living" in their dashboard; we want the platform to work perfectly in the background. This 9-tier stack ensures that even during massive traffic spikes (e.g., Black Friday campaigns), the core API remains responsive due to the isolation of TTS and Analytical (Observe) workloads.

---

---

## 8. Specialized Infrastructure & Storage (Sovereign Tools)

To support advanced remote-access scenarios and high-performance object storage requirements, the following sovereign tools are integrated:

- **Virtual Browser:** **[Neko](https://github.com/m1k1o/neko)** — A self-hosted virtual browser that runs in Docker and uses WebRTC. Used for isolated browsing, multi-user collaboration, and secure previewing of web content within the infrastructure.
- **High-Perf Object Storage:** **[RustFS](https://github.com/rustfs/rustfs)** — An open-source, S3-compatible high-performance object storage system written in Rust. Optimized for small object payloads (4KB) and designed for migration/coexistence with other S3-compatible platforms.

---
*Last Updated: April 2026 | Following the BlackLoverTech "Build to Learn" Mandate.*
