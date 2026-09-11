# 🏛️ Foundations: SaaS Standards
> **BlackLoverTech Industrial Standard** | April 2026
> [← Master Index](file:///d:/brain/source/Master_Source_Index.md) | Status: **Sovereign**

This document synthesizes the **Master Engineering Stack** and **Technical Engineering Standards** of the Whatomate/BlackLoverTech ecosystem. It codifies the "Common Sense Scaling" philosophy, drawing from Zerodha's engineering DNA and the "Builder's Ideology": *Don't learn to build. Build to learn.*

---

## 🏛️ 1. Core Architectural Strategy

### Mechanical Sympathy & Sovereign Scaling
We select languages and runtimes based on **utility and deterministic performance**, avoiding "coolness" for "durability."

| Layer | Technology | Infrastructure Role |
|---|---|---|
| **Core API** | Go + `fasthttp` / `fastglue` | Single binary, goroutine concurrency, sub-millisecond response. |
| **Data Logic** | Go + `easyjson` / `stuffbin` | Reflection-free JSON & embedded binary assets for extreme portability. |
| **High-Perf Systems** | Rust | Zero-cost abstractions, memory safety, systems-level efficiency (Mailing/Blob). |
| **Low Latency** | Zig / C | Manual memory management for bare-metal tasks (PDF signing, transcode). |
| **Data & Glue** | Python + `boto3` | Treatment of AWS as "Dumb Plumbing" (S3/EC2 primitives only). |
| **Frontend** | Vue 3 + TypeScript | "4-Side Industrial Shell" architecture (IBM Plex + Local Fonts). |
| **Mobile** | Flutter (Impeller 2.0) | AOT → ARM, GPU-direct rendering, zero JS bridge. |

### The "Boring > Cool" Mandate (OS-Level)
- **File System**: **XFS** is mandatory for database volumes. Its allocation group architecture allows superior parallel I/O compared to Ext4.
- **TCP Tuning**: Standardized sysctls for high-concurrency: `net.core.somaxconn=1024`, `net.ipv4.tcp_max_syn_backlog=4096`, `net.ipv4.tcp_tw_reuse=1`.
- **IO Scheduler**: Use `mq-deadline` or `none` for NVMe/SSD storage to minimize kernel overhead.
- **Stuffbin Optimization**: All static assets (HTML/CSS/Config) must be compressed with **Zstd** and packed into the Go binary using `stuffbin`.

---

## 🏗️ 2. The 20-Tier Cluster Topology

A deterministic map of containerized services that constitute the platform's sovereign infrastructure.

| Tier | Role | Primary Image |
|---|---|---|
| **T1 Edge** | Nginx / Cloudflare Ingress | `nginx:alpine` |
| **T2 Identity** | Zitadel / Authentik (OIDC) | `zitadel:latest` |
| **T3 Command** | Core Business Engine (Go) | `alpine:go` |
| **T4 Worker** | Async Task/Webhook Workers | `alpine:go` |
| **T5 Memory** | HA Redis (Cache/Locks) | `redis:alpine` |
| **T6 Truth** | Hardened PostgreSQL (RLS) | `postgres:alpine` |
| **T7 Analysis** | Vector → ClickHouse (Loj) | `timberio/vector` |
| **T8 Intelligence** | Qdrant (Vector/RAG) | `qdrant:latest` |
| **T9 Governance** | HashiCorp Vault (Secrets) | `vault:alpine` |
| **T10 Media** | MinIO (Object Storage) | `minio/minio` |
| **T11 Pulse** | NATS.io (Events/JetStream) | `nats:alpine` |
| **T12 Search** | Meilisearch / Typesense | `meilisearch` |

---

## 🛠️ 3. Core Software Components

### API & Persistence Standards
- **Standard Protocol**: High-throughput `fasthttp` wrapper (`fastglue`) for sub-millisecond API response.
- **SQL Governance**: We use **`goyesql`** to keep raw SQL logic in `.sql` files, separated from Go code. Mandatory `EXPLAIN ANALYZE` comments for every query.
- **Schema Evolution**: **Atlas (Tier 19)** for declarative migrations. No manual `ALTER` scripts; desired state is audited in Git.
- **Serialization**: `ProtoBuf` for internal gRPC; `easyjson` for reflection-free JSON marshaling in public APIs.
- **Database Rules**: Monthly logical partitioning for `messages` and `webhooks`. Row-Level Security (RLS) keyed to `tenant_id`.

### Code-to-Doc Parity (CI Enforcement)
The **Error Taxonomy** is an enforceable contract.
- **Mandatory Taxonomy**: Every error must match an `ERR_` code string documented in [Security & Gov](file:///d:/brain/source/Security_Governance_Protocol.md).
- **CI Gate**: A linter (`errlint`) verifies that every `SendErrorEnvelope` call uses a registered code. Undocumented codes break the build.

---

## 🎨 4. Product & UX Philosophy

### "User Disengagement"
Build for **Utility**, not "Engagement." The goal is for users to solve their problem as fast as possible and then leave the app.
- **No Dark Patterns**: No rate-popup, no hidden unsubscribe, no red-dot cues.
- **No-Spinner Mandate**: CSS shimmer skeletons only. Interaction target: < 100ms.
- **The Shell UI**: Zoho-inspired navigation. Fixed 4-Side Frame (TopBar, LeftNav, RightDrawer, BottomStatus).

---

## 🛡️ 5. Security & Sovereignty
- **Zero-Trust**: Mandatory mTLS for internal communication. JIT access for admin actions.
- **Multi-Tenant Hub**: PostgreSQL **Row-Level Security (RLS)** is non-negotiable.
- **Secrets Management**: No environment variables in production. Vault injects read-only files at `/run/secrets/`.

---
*Created by Antigravity | Mechanical Sympathy — BlackLoverTech*
> [↑ Return to Command Center](file:///d:/brain/source/Master_Source_Index.md)
