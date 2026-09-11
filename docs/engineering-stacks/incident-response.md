# 🚨 Incident Response Engineering Stack

This document defines the protocols, playbooks, and systems for detecting, mitigating, and documenting system failures within the Whatomate ecosystem.

## 1. Monitoring & Alert Routing

We utilize a unified observability stack (OTel + ClickHouse T7) to source high-fidelity alerts.

- **Thresholds:** Anomalies (e.g., Error Rate > 2%, Latency p95 > 1s) trigger an alert.
- **Routing:**
  - **Critical:** PagerDuty (Voice Call) + High-priority Slack channel.
  - **Warning:** Slack only; tracked for 24h trends.
- **Filtering:** Alertmanager suppresses flapping alerts and groups related events (e.g., DB Down → suppresses all App-level connection errors).

## 2. Playbooks: Circuit Breakers & Failovers

Operational reliability is maintained through automated and manual "Circuit Breaker" switches.

- **Upstream Failures (Meta/Stripe):** If Meta returns `5xx` for >60s, the Campaign Engine (T4) enters "Pause-Retry" mode automatically. 
- **Database Load:** If T6 connection pool exceeds 90% saturation, the Gateway (T1) begins shedding non-essential traffic (Analytics, Profile Updates) while prioritizing Messaging.
- **Manual Kill Switch:** Accessible via the Admin Hub (T15), allowing immediate platform-wide suspension of background jobs.

## 3. Runbooks (The "How-To")

Standardized documents located in `/docs/runbooks/` for common failure modes:

| Scenario | Primary Action | Tools |
|---|---|---|
| **Redis Flush** | Scale T5 to 3 nodes; verify persistence RDB/AOF. | `redis-cli`, `coolify` |
| **Vault Seal** | Retrieve Unseal Keys from 3/5 physical guardians. | `vault operator unseal` |
| **NATS Backlog** | Horizontal scale T4 Workers; verify Consumer Lag. | `nats sub`, `signoz` |

## 4. Chaos Drills & Resilience Testing

Following the "Quality & Chaos" standard, we run quarterly drills (Game Days).

- **Schedule:** 2nd Tuesday of every quarter.
- **Scenarios:** "Region Down", "SSO Certificate Expired", "Ransomware Simulation".
- **Tooling:** Chaos Mesh (Kubernetes) and manual pod kills during off-peak hours.

## 5. Postmortem Template

Every `CRITICAL` incident requires a Blameless Postmortem within 48h.

- **Summary:** What happened? (Timeline).
- **Impact:** How many tenants/users were affected?
- **Root Cause:** Why did it happen? (The 5 Whys).
- **Corrective Actions:** How do we prevent this forever? (Linked to a GitHub Issue).
- **Outcome:** Postmortem is published to the internal Engineering Wiki.

## 6. Implementation Example (Retry logic)

```go
func ReliableExecute(fn func() error) error {
    backoff := 100 * time.Millisecond
    for i := 0; i < 3; i++ {
        if err := fn(); err == nil {
            return nil
        }
        time.Sleep(backoff)
        backoff *= 2 // Exponential backoff
    }
    return ERR_SYSTEM_CIRCUIT_BROKEN
}
```

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard*
