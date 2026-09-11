# 🔨 Universal SaaS: Quality & Chaos Engineering Stack

This document defines the **Forge Layer** of the platform—synthesizing **Industrial Chaos Engineering**, **High-Velocity Performance Testing**, and **Automated Quality Governance**. It is the blueprint for delivering "Zerodha-Grade" reliability at scale.

---

## 🏛️ 01. Chaos Mesh: Resilience by Design

We don't wait for failure; we inject it.

- **Network Torture**: We use **Chaos Mesh** to periodically inject network latency (1s-5s) and packet loss (5%-10%) between the `app` and `redis`. 
- **The "Pod-Killer" Protocol**: Automated, random killing of 5% of application pods during peak hours to ensure the platform's **Anti-fragility** (Self-healing and rapid failover).
- **DNS Poisoning Tests**: Simulating a DNS failure to verify that internal services fail-over correctly to IP-cached fallbacks.

---

## 🏎️ 02. Performance Engineering (k6 & Vegeta)

We treat performance as a regression bug.

- **Baseline Load Testing (CI)**: Every merger into `main` must pass a **k6 baseline test** simulating 1,000 concurrent WhatsApp webhooks. If p95 latency increases by > 10ms, the build is rejected.
- **Stress-to-Break (Stress Testing)**: Quarterly **Vegeta** attacks to identify the "Breaking Point" of the 9-tier stack. 
    - **Industrial Target**: Support 10,000 transactions per second (TPS) on a 3-node cluster.
- **Leak Detection**: 24-hour "Soak Tests" to identify slow memory leaks in the Go runtime and Redis connection pool growth.

---

## 🤖 03. Automated Quality Governance

Quality is a mandatory CI/CD gate, not an afterthought.

- **The "Unbreakable" Unit Test**: 85% mandatory code coverage. Tests MUST cover edge cases defined in the `error_taxonomy.md` (e.g., simulating a Juspay gateway timeout).
- **Contract Testing**: Using **Pact** to ensure that changes in the `app` API do not break the `frontend` or the `worker` services (preventing upstream/downstream drift).
- **Identity-First E2E**: Automated Playwright/Cypress tests that verify the full "Login -> Payment -> Message Sent" journey across the 4-side shell interface.

---

## ✅ 04. The "Definition of Done" (DoD) Contract

A feature is only "Done" when it meets the **Universal Engineering Standard**:

1.  **Observability**: New handlers must have OTel tracing and at least 3 Zap-log entries.
2.  **Taxonomy**: All error states must be documented in the Markdown spec.
3.  **Mechanical Sympathy**: Core loops must have < 50ms execution time.
4.  **Security**: Image must pass a zero-vulnerability Trivy scan.
5.  **Performance**: k6 load test results must be within 5% of the baseline.

---

## 📈 05. Quality-of-Service (QoS) Multi-tenancy

We ensure "Noisy Neighbor" isolation during high-load events.

- **Weighted Fair Queuing**: In the `worker` service, high-value "Pro" tenants are processed in specialized priority queues.
- **Resource Cgroups**: Docker-level limits on CPU/RAM for specific tenant-affinity pods, ensuring one tenant's heavy batch job cannot take down the platform for others.

---
*Last Updated: April 2026 | Following the Universal SaaS Engineering Standard.*
