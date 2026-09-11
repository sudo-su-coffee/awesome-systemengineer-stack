# 🏛️ Universal SaaS — Master Engineering Stack Reference
> Condensed from 19 spec files | April 2026 | BlackLoverTech / Whatomate

---

## 1. Core Language & Runtime

| Layer | Choice | Why |
|---|---|---|
| **Backend API** | Go + `fasthttp`/`fastglue` | Single binary, goroutine concurrency, near-native perf |
| **Systems / Crypto** | Rust | Zero-cost abstractions, no GC, memory safety |
| **Ultra-low latency** | Zig | Manual memory for bare-metal tasks (PDF signing) |
| **Data / Glue** | Python + boto3 | AWS orchestration, data parsing, automation |
| **Frontend** | Vue 3 + TypeScript | Composition API, Pinia, TanStack Query |
| **Mobile** | Flutter (Dart/Impeller) + Kotlin/Swift bridges | AOT → ARM, GPU-direct rendering, zero JS bridge |
| **Serialization** | ProtoBuf (binary) + `easyjson` (Go) | 80% smaller payloads vs JSON |
| **Compression** | Zstd (Meta) | 5× faster than Gzip |

**OS Sympathy:** XFS for DB volumes, tuned TCP stack (`somaxconn`, `tcp_max_syn_backlog`), `mq-deadline` scheduler on NVMe.

---

## 2. The 20-Tier Cluster Map

| Tier | Namespace | Role | Image |
|---|---|---|---|
| T1 Edge | `gateway` | Kong / Nginx Ingress (only public-facing service) | `kong:latest` |
| T2 Identity | `auth-api` | Zitadel / Keycloak IAM ([Detail](file:///d:/github/wt/auth_sso_engineering_stack.md)) | `keycloak/keycloak:latest` |
| T3 Command | `core-app` | Go Business Engine | `alpine:go` |
| T4 Worker | `task-worker` | Async messaging & webhook ingestion | `alpine:go` |
| T5 Memory | `cache-redis` | HA Redis (rate-limits, pub/sub, locks) | `redis:alpine` |
| T6 Truth | `primary-db` | Hardened PostgreSQL | `postgres:alpine` |
| T7 Analysis | `observe-ck` | Vector / FluentBit → ClickHouse telemetry | `timberio/vector:latest` |
| T8 Intelligence | `vector-db` | Qdrant (RAG/AI embeddings) | `qdrant/qdrant:latest` |
| T9 Governance | `vault-secrets` | HashiCorp Vault root-of-trust | `vault:alpine` |
| T10 Media | `object-store` | MinIO S3-compatible storage | `minio/minio:latest` |
| T11 Pulse | `msg-stream` | NATS.io Streaming & Workflow ([Detail](file:///d:/github/wt/workflow_automation_engineering_stack.md)) | `nats:alpine` |
| T12 Search | `search-engine` | Meilisearch Instant Search ([Detail](file:///d:/github/wt/search_discovery_engineering_stack.md)) | `getmeili/meilisearch:latest` |
| T13 Decoy | `honeypot-bait` | Cowrie / Dionaea active deception | `cowrie/cowrie:latest` |
| T14 Shield | `runtime-ids` | Falco / CrowdSec runtime security | `falcosecurity/falco:latest` |
| T15 Ops | `admin-hub` | Portainer EE / Dozzle | `portainer/portainer-ee:latest` |
| T16 Mesh | `overlay-vpn` | Tailscale / Headscale | `tailscale/tailscale:latest` |
| T17 Content | `media-engine` | Imaginary / FFmpeg media processing | `nextcloud/imaginary:latest` |
| T18 Law | `policy-opa` | Open Policy Agent governance-as-code | `openpolicyagent/opa:latest` |
| T19 Evolution | `schema-ops` | Atlas Schema Migration ([Detail](file:///d:/github/wt/schema_migration_engineering_stack.md)) | `arigaio/atlas:latest` |
| T20 Eternal | `disaster-dr` | Velero / Restic business continuity | `velero/velero:latest` |

**3-Layer Shield:** DMZ (T1, T13, T14) → App Mesh (T3, T4, T5, T11, T17, T18) → Data Vault (T6, T8, T9, T10, T12, T15, T16, T19, T20).

**Container Standard:** Multi-stage builds with `distroless` runtime. Zero root processes. Trivy scan blocks `HIGH`/`CRITICAL` in CI.

---

## 3. Economic Engine (Isolated Suite)

| Layer | Service | Role |
|---|---|---|
| Orchestrator | Hyperswitch (Juspay) | Smart payment routing: Razorpay → Paytm → Stripe failover |
| Metering | Lago | Real-time Usage & Billing ([Detail](file:///d:/github/wt/billing_subscription_engineering_stack.md)) |
| Visibility | Metabase | MRR/Churn/LTV dashboards, reconciliation |

**Ledger rules:** Redis-Lua atomic locking (no double-spend), double-entry mandate, UUIDv7 transaction IDs, Juspay Vault for token portability.  
**Tax:** Avalara/TaxJar hooks for GST/VAT; Typst (Rust) for invoices in <50ms.  
**Pine Labs:** Physical POS → Hyperswitch webhook → Lago ledger hydration in <500ms (P2PE + PCI-PTS).

---

## 4. AI & LLM Layer

- **Retrieval:** Hybrid search — Qdrant (dense vectors) + BM25 (sparse keywords) merged via Reciprocal Rank Fusion → Cohere/BGE cross-encoder re-ranking.
- **Caching:** Prefix caching at provider level (up to 50% TTFT reduction) + Redis vector cache (>0.98 similarity threshold = instant return).
- **Safety:** Every vector query MUST include `tenant_id` filter. High-sovereignty tenants get dedicated Qdrant namespaces.
- **Observability:** Ragas LLM-as-Judge scoring (Groundedness, Faithfulness, Relevance) + ClickHouse trace per response (chunks, prompt version, latency).
- **Cost:** Redis fixed-window token quotas per tenant. Fallback: Opus → Haiku on rate limit.

---

## 5. Data Lakehouse

- **CDC:** Debezium on PostgreSQL WAL → NATS.io (T11) → <1000ms sync to ClickHouse.
- **Storage:** Apache Iceberg on MinIO — ACID, schema evolution, time-travel queries.
- **Multi-tenancy:** Data partitioned by `tenant_id`; Trino/DuckDB RLS enforced at compute layer. Zero raw PII in lake.
- **Tiered Aging:** Hot (0–90d) → NVMe ClickHouse; Cold (90d+) → MinIO S3.
- **Metrics:** Pre-computed hourly/daily ClickHouse aggregates feed Metabase. No direct production DB queries for reporting.

---

## 6. Frontend (Vue 3 / 4-Side Shell)

**Shell (persistent, no reflow):**
- Top 48px — ⌘K global search, org switcher
- Left 52px — icon nav rail, amber active indicator
- Right 64px — context drawer (no URL change)
- Bottom 32px — API latency, Redis health, quotas

**Design tokens:** `#080a0f` void background, IBM Plex Sans/Mono (self-hosted only, no CDN).  
**No-spinner mandate:** CSS shimmer skeletons only. Virtualization required for lists >100 rows.  
**Performance targets:** FCP <800ms, interaction <100ms, bundle <250kb gzip.  
**Stack:** Vue 3 + Pinia + TanStack Query + Reka-UI + Zod + Lucide + Tailwind (utility-only).  
**ETag/304:** All semi-static GET responses use ETag; server returns 304 on cache hit → zero re-download.

---

## 7. Mobile Stack

- **UI Engine:** Flutter / Impeller (GPU-direct, AOT ARM, zero JS bridge)
- **Data Rails:** gRPC/ProtoBuf (primary) → `flutter_rust_bridge` (heavy compute in Rust thread) → GraphQL (analytics only)
- **Local DB:** Isar (Rust/Dart, ACID) + Hive (KV cache)
- **State:** Riverpod (compile-time safe, no BuildContext dependency)
- **Payments:** Hyperswitch Mobile SDK (single SDK replaces Razorpay + Stripe)
- **Hardening:** SSL pinning on all gRPC, biometric guard for sensitive actions
- **OTA:** Shorebird.dev (Flutter code-push) & Coolify GitOps ([Detail](file:///d:/github/wt/ci_cd_gitops_engineering_stack.md))
- **Observability:** Sentry (self-hosted crashes), Umami (privacy-first analytics), Pyroscope (CPU/memory profiling)
- **Size target:** APK <20MB. No reflection. All non-UI work on Dart Isolates.

---

## 8. WhatsApp Platform (Cloud API)

**Current stable API:** `v21.0` — pin in config, bump only as a planned release.

**Key IDs:**
- `WABA_ID` — tenant mapping (webhook `entry.id`)
- `PHONE_NUMBER_ID` — used in all send calls (`/{id}/messages`)
- `WA_MESSAGE_ID` — de-dupe and status tracking
- `BSUID` — 2026 rollout: replaces phone in `contacts[].wa_id` for username-opted users; use `recipient` key not `to`

**Core SaaS → WA mapping:**

| Feature | Mechanism |
|---|---|
| Inbox | Webhooks (in) + `/messages` (out) |
| Message status | `statuses[]` — sent/delivered/read/failed/played |
| Templates | Required outside 24h customer service window |
| Media inbound | Receive `media.id` → download → MinIO → CDN |
| Flows | `interactive.type: "flow"` → receive `nfm_reply` |
| Calling | Permission template + `/calls` + calling webhooks |
| Campaigns | MM API (`/{PHONE_NUMBER_ID}/marketing_messages`) |
| Welcome/Ice Breakers | Conversational Components API → `request_welcome` webhook |

**Tokens:** System User tokens preferred (non-expiring). Store in Vault (T9). Rotate on breach. WABA-level `webhook_verify_token` per tenant.

**24h Window Rule:** Template required for business-initiated messages outside the customer service window. Utility templates are free in India as of Q1 2025.

**Template Pacing:** Meta auto-paces new/paused templates at 1K/day; scales on quality. Never delete+recreate to reset — use TTL customization instead.

**Webhooks — reliability rules:**
1. Respond `200 OK` in <5s (offload processing async via NATS T11)
2. De-duplicate on `WA_MESSAGE_ID` before processing
3. Log `fbtrace_id` / `wamid` on every error for Meta escalation
4. Subscribe fields: `messages`, `message_template_status_update`, `account_review_update`

---

## 9. Security & Compliance

**Zero-Trust:** mTLS between all internal services. OIDC at gateway; core app only receives pre-authenticated requests.

**Cryptography:**
- Passwords: Argon2id (64MB memory, 3 iterations, 128-bit salt + 256-bit pepper in Vault)
- Signatures: Ed25519 (API tokens, webhook sigs)
- Hardware handshakes: RSA-4096
- Key rotation: 90-day cycle via KMS/Vault; app never sees root key

**WAF:** OWASP Top 10 rules at T1. Redis rate limits: 60 RPM (public), 5000 RPM (agents), webhook IPs validated against Meta's published range.

**Multi-tenant isolation:** PostgreSQL RLS on every table keyed to `tenant_id` — physically impossible for cross-tenant data leak.

**Secrets:** Zero env vars in production. Vault injects read-only files at `/run/secrets/` at bootstrap.

**Compliance:**
- India DPDP: payment data pinned in AWS Mumbai
- EU GDPR: DPA at platform level, "Right to be Forgotten" DAG (identity scrub → ledger purge → media shred → lakehouse hard-delete → cryptographic deletion certificate)
- US SOC2 Type II: JIT access requests, immutable ClickHouse audit logs, `SUPER_USER_ACCESS` events visible to tenant

**PII in logs:** Replaced with UUIDs/masked strings by default. CMK support for enterprise tenants.

---

## 10. Observability & Reliability

**Stack:** OTel Collector → ClickHouse (logs + traces + metrics unified). Grafana / Jaeger for visualization.

**4 Golden Signals:** Latency (p50/p95/p99, target <100ms core API) | Traffic (RPS, queue depth) | Errors (mapped to `ERR_` taxonomy) | Saturation (CPU/RAM, connection pools).

**Tracing:** `trace_parent` header propagated Nginx → App → Worker → Redis. Every AI response linked to ClickHouse trace ID.

**SLO:** 99.9% WhatsApp delivery success. Error budget breach → freeze feature dev. ([Incident Response](file:///d:/github/wt/incident_response_engineering_stack.md))

**Reliability patterns:**
- Circuit breaker on upstream failures → instant `ERR_UPSTREAM_DOWN` (no goroutine stall)
- Exponential backoff with jitter on retries
- Mandatory `HEALTHCHECK` in every Dockerfile
- JSON logs to stdout (no disk I/O); pprof on private `/debug` port

---

## 11. Quality & Chaos Engineering

- **Chaos Mesh:** Periodic latency injection (1–5s) + 5% pod-kill during peak hours + DNS failure simulation
- **Load testing (CI gate):** k6 baseline at 1,000 concurrent webhooks on every merge; >10ms p95 regression = build failure
- **Stress target:** 10,000 TPS on 3-node cluster (quarterly Vegeta)
- **Coverage:** 85% mandatory; edge cases must cover `error_taxonomy.md` scenarios
- **Contract testing:** Pact between `app`/`frontend`/`worker` to prevent API drift
- **E2E:** Playwright full "Login → Payment → Message Sent" journey

**Definition of Done:** OTel tracing + 3 Zap logs + error taxonomy entry + Trivy zero-vuln scan + k6 within 5% baseline + <50ms core loop execution.

---

## 12. Hardware / IoT / POS

- **Security:** mTLS (X.509 per device), TPM/TEE key storage, signed firmware (RSA/ECC), PCI-PTS anti-tamper mesh + active key erasure on breach
- **Binary efficiency:** ProtoBuf (80% smaller), differential sync, delta OTA patches
- **Soundbox:** MQTT-triggered audio, pre-cached fragments on NOR Flash, remote language swap
- **Offline mode:** Local tax engine (GST/VAT), SQLite/LittleFS encrypted queue, Batch & Forward on reconnect
- **Fleet ops:** QR zero-touch pairing, Blue/Green OTA (5% canary → rollback on health drop), Metabase heartbeat dashboard
- **Hardware lifecycle (RMA):** State machine: `FAULTY → TRIAGE → REPLACE → RETIRED` with encrypted wipe before return
- **Anti-theft:** Geo-lock — >1km from registered GPS = instant secure enclave lockdown + key erasure
- **Predictive maintenance:** Battery <75% → auto-ticket before failure

---

## 13. Marketplace / Plugin Ecosystem

- **Sandbox:** Wasm (Wasmtime/Wasmer) inside T3. Strict CPU + 50MB RAM quota. Instant kill on breach.
- **I/O:** Plugins use Whatomate ABI host-functions only — no direct network/filesystem access
- **Auth:** Short-Lived Access Tokens (SLATs) with scoped permissions; never expose platform secrets
- **Governance:** SAST + manual review for every published plugin. Version-pinned installs.
- **Audit:** Every plugin action logged as `ACTION_EXTENSION` in T7

---

## 14. White-labeling & i18n

- **Theming:** CSS custom properties (`--brand-primary`, `--bg-primary`, etc.) injected per-tenant via dynamic `<style id="tenant-theme">` on handshake. Vue auto-reflects — no reload.
- **Domains:** CNAME mapping with auto Let's Encrypt via Cert-Manager. Logos served from MinIO, cached at Nginx edge.
- **i18n:** Vue-I18n with lazy-loaded chunks. RTL via CSS logical properties. UTC storage → `Intl.DateTimeFormat` client-side.
- **A11Y:** WCAG 2.1 Level AA. Semantic HTML5. CI contrast-ratio check (4.5:1 minimum). Dark-mode-by-default.

---

## 15. Error Taxonomy (`ERR_` System)

**Format:** `ERR_[DOMAIN]_[REASON]` — replaces ambiguous HTTP codes.

**Code ranges:**
- `1000–1999` WhatsApp / Meta
- `2000–2999` Payments / Hyperswitch
- `3000–3999` Auth / SSO / Tenancy
- `4000–4999` DB / ClickHouse / Infra
- `5000–5999` AI / Chatbot / Flows
- `6000–6999` Campaigns
- `7000–7999` Calling / SIP / IVR
- `8000–8999` CRM / Contacts / ETL
- `9000–9999` System Health / Security

**CI enforcement:** Any `ERR_` code used in Go but absent from the taxonomy doc = failed build.

**Log rules:**
1. All errors → ClickHouse with `error_code` indexed
2. `CRITICAL` errors → immediate Slack/PagerDuty
3. Third-party errors (Meta, Stripe) → log their `fbtrace_id`/trace in payload

**Response schema:** `{ success, error_code, message, domain, grpc_status, meta_trace_id, actionable, details }`

**3 developer rules:** Never return stack traces to client. Idempotency on webhooks/payments. Set `actionable: true` when UI must prompt the user.

---

## 16. Self-Hosted Operational Registry

| Category | Tool | Notes |
|---|---|---|
| PaaS | Coolify | Docker-based, replaces Heroku/Vercel |
| Auth / SSO | Zitadel or Authentik | OIDC/SAML, Go-based |
| BI | Metabase | Primary BI (Zerodha standard) |
| Search | Meilisearch / Typesense | C++ instant search |
| Vector DB | Qdrant (Rust) | AI semantic routing |
| Feature Flags | GrowthBook | Decouple deploy from release |
| CRM / Support | Chatwoot (Rails) | WhatsApp-native support |
| Internal Chat | Zulip (Python) | Threaded, Zerodha standard |
| Secrets UI | Vaultwarden (Rust) | Bitwarden-compatible |
| Project Mgmt | Plane | Open-source Linear/JIRA |
| Observability | SigNoz | Go/TS, unified logs/metrics/traces |
| Uptime | Uptime Kuma | Public status page |
| Admin Portals | Appsmith | Internal tooling (support, refunds) |
| Workflow Automation | n8n | Webhook → CRM wiring |
| Git | Forgejo | Self-hosted, 100% control |
| Analytics | Umami | Privacy-first, no PII |
| Mobile CI | oore.build (Rust) | Flutter CI + OTA distro |
| Maps | MapLibre | No Google tracking |

---

## 17. 📄 Specialized Deep-Dive References

To keep the master stack lean, technical implementation details are offloaded to specialized spec files:

| Domain | Specification File |
|---|---|
| **Identity** | [auth_sso_engineering_stack.md](file:///d:/github/wt/auth_sso_engineering_stack.md) |
| **Workflows** | [workflow_automation_engineering_stack.md](file:///d:/github/wt/workflow_automation_engineering_stack.md) |
| **Messaging & Push** | [email_sms_notification_engineering_stack.md](file:///d:/github/wt/email_sms_notification_engineering_stack.md) |
| **Discovery** | [search_discovery_engineering_stack.md](file:///d:/github/wt/search_discovery_engineering_stack.md) |
| **GitOps & CI** | [ci_cd_gitops_engineering_stack.md](file:///d:/github/wt/ci_cd_gitops_engineering_stack.md) |
| **Migrations** | [schema_migration_engineering_stack.md](file:///d:/github/wt/schema_migration_engineering_stack.md) |
| **CRM & SLA** | [crm_contacts_engineering_stack.md](file:///d:/github/wt/crm_contacts_engineering_stack.md) |
| **Bulk Pacing** | [campaigns_bulk_messaging_engineering_stack.md](file:///d:/github/wt/campaigns_bulk_messaging_engineering_stack.md) |
| **Billing & Metering** | [billing_subscription_engineering_stack.md](file:///d:/github/wt/billing_subscription_engineering_stack.md) |
| **Onboarding** | [multitenancy_onboarding_engineering_stack.md](file:///d:/github/wt/multitenancy_onboarding_engineering_stack.md) |
| **Incidents** | [incident_response_engineering_stack.md](file:///d:/github/wt/incident_response_engineering_stack.md) |

---

*Last Updated: April 2026 | Universal SaaS Engineering Standard — BlackLoverTech*
