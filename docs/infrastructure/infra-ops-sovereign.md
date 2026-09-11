# 🏗️ Infrastructure & Operations Sovereign
> **BlackLoverTech Industrial Standard** | April 2026
> [← Master Index](file:///d:/brain/source/Master_Source_Index.md) | Status: **Sovereign**

This document defines the **Infrastructure & Operations (InfraOps)** layer—a unified framework for orchestrating the Whatomate/BlackLoverTech 20-tier cluster with industrial-grade reliability, GitOps automation, and proactive chaos engineering.

---

## 🏛️ 1. Cluster Orchestration & Container Standards

### The 20-Tier Topology
The platform is decomposed into 20 specialized, isolated namespaces to ensure absolute fault isolation and vertical scaling.

| Tier | Namespace / Service | Industrial Role | Runtime |
|---|---|---|---|
| **T1: Edge** | `gateway` | Kong / Nginx Proxy Manager Ingress. | `kong:latest` |
| **T2: Identity** | `auth-api` | Zitadel / Authentik (OIDC) / Keycloak. | `zitadel:latest` |
| **T3: Command** | `core-app` | High-performance Go Business Engine. | `distroless` |
| **T4: Worker** | `task-worker` | Async messaging & webhook ingestion. | `distroless` |
| **T5: Memory** | `cache-redis` | High-Availability Redis State & Locks. | `redis:alpine` |
| **T6: Truth** | `primary-db` | Hardened PostgreSQL (Core Persistence). | `postgres:alpine` |
| **T7: Analysis** | `observe-ck` | Vector / FluentBit telemetry to ClickHouse. | `timberio/vector` |
| **T8: Intelligence**| `vector-db` | Qdrant Vector store for RAG/AI retrieval. | `qdrant:latest` |
| **T9: Governance** | `vault-secrets` | HashiCorp Vault Root-of-Trust (mTLS, Keys). | `vault:alpine` |
| **T10: Media** | `object-store` | MinIO S3-compatible Object Storage. | `minio/minio` |
| **T11: Pulse** | `msg-stream` | NATS.io JetStream Streaming & RPC. | `nats:alpine` |
| **T12: Search** | `search-engine`| Meilisearch Instant UX Search Kernel. | `meilisearch` |
| **T13: Decoy** | `honeypot-bait`| Cowrie / Dionaea Active Deception. | `cowrie/cowrie` |
| **T14: Shield** | `runtime-ids` | Falco / CrowdSec Runtime Security. | `falcosecurity/falco`|
| **T15: Ops Hub** | `admin-hub` | Portainer / Dozzle / Uptime Kuma. | `portainer:latest` |
| **T16: Mesh** | `overlay-vpn`| Tailscale / Headscale Mesh Overlay. | `tailscale:latest` |
| **T17: Content** | `media-engine`| Imaginary (Go) / FFmpeg processing. | `nextcloud/imaginary`|
| **T18: Law** | `policy-opa` | Open Policy Agent (OPA) Governance. | `opa:latest` |
| **T19: Evolution**| `schema-ops` | Atlas / Liquibase Schema Migration. | `arigaio/atlas` |
| **T20: Eternal** | `disaster-dr` | Velero / Restic backup to offsite S3. | `velero/velero` |

### 💎 The 3-Layer Shield Networking
- **DMZ (Layer 1)**: Only T1 (Gateway), T13 (Decoy), and T14 (Shield) are public-facing.
- **App Mesh (Layer 2)**: Core compute (T3, T4, T5, T11) resides on an internal overlay network.
- **Data Vault (Layer 3)**: Persistent state (T6, T8, T9, T10, T12) is physically isolated.

---

## 🚀 2. GitOps & CI/CD Delivery

### Portable Pipelines (Dagger)
We use **Dagger (Go-based)** to define pipelines as code.
- **Zero-Dependency**: Pipelines run identically on local machines and Forgejo runners.
- **Build Gate**: PRs trigger Lint, Unit Tests, Trivy Scans, and k6 performance benchmarks.

### Continuous PaaS (Coolify)
- **Deployment Flow**: Forgejo Webhook → Coolify Hook → Docker Build → Blue/Green Swap.
- **Blue/Green Strategy**: T3/T4 updates spin up a "Green" stack; health is verified before the T1 (Gateway) swap.

---

## 📈 3. Observability & The "ClickStack"

### Unified Telemetry Backend
We utilize **The ClickStack** (OTel + ClickHouse) for logs, metrics, and traces.
- **Gateway**: OpenTelemetry Collector sidecars sink OTLP data into ClickHouse.
- **Distributed Tracing**: `trace_parent` propagation across Nginx → Go App → Worker → Redis.
- **4 Golden Signals**: Mandatory tracking of Latency (p95 < 100ms), Traffic (RPS), Errors, and Saturation.

---

## 🚨 4. Incident Response & SRE Protocols

### Alert Routing & Thresholds
- **Critical Anomalies**: Error Rate > 2% or Latency p95 > 1s trigger PagerDuty voice calls.
- **Circuit Breakers**:
  - **Upstream**: Automated "Pause-Retry" if Meta/Stripe returns 5xx for > 60s.
  - **Shedding**: Gateway sheds non-essential traffic (Analytics) if DB saturation > 90%.

### Blameless Postmortems
Every `CRITICAL` incident requires a postmortem within 48h, answering **"The 5 Whys"** and linking corrective actions to the GitHub/Forgejo issue tracker.

## 💾 5. Disaster Recovery & Continuity (Tier 20)

### Velero / Restic Strategy
We ensure physical data continuity across availability zones.
- **Snapshot Frequency**: Tier 6 (Truth) and Tier 10 (Media) volumes are snapshotted every 4 hours.
- **Offsite Replication**: Snapshots are encrypted and synced to an offsite S3-compatible provider (e.g., Backblaze B2 or Wasabi) using **Velero**.
- **The "Burn Test"**: Full platform recovery drills are performed quarterly to verify that T1-T20 can be restored to a clean cluster in < 30 minutes.

---

## 🔨 5. Quality & Chaos Engineering

### Chaos by Design (Chaos Mesh)
- **Network Torture**: Periodic injection of 1s-5s latency and 5%-10% packet loss.
- **Pod-Killer**: Randomly killing 5% of application pods during peak hours.

### Performance as a Bug
- **Baseline CI Check**: Every merge must pass a k6 test (1,000 concurrent webhooks). p95 regression > 10ms = Build Failure.
- **Stress Target**: 10,000 TPS on a 3-node cluster.

---
*Created by Antigravity | Mechanical Sympathy — BlackLoverTech*
> [↑ Return to Command Center](file:///d:/brain/source/Master_Source_Index.md)
