# 🚀 CI/CD & GitOps Engineering Stack

This document defines our continuous integration and delivery philosophy, ensuring reliable, automated, and secure deployments.

## 1. Pipelines with Dagger

We use **Dagger** to define our CI/CD pipelines as code (using Go). This ensures that pipelines run identically on a developer's local machine as they do in the cloud.

- **Portable:** Zero dependency on specific CI runner environments (PostgreSQL/Redis are run as ephemeral containers via Dagger).
- **Caching:** Dagger's fine-grained layer caching reduces build times by >50%.
- **Validation:** Every merge request triggers a Dagger run covering Lint, Unit Tests, and k6 Load Tests.

## 2. Source Control & Runners (Forgejo)

We standardize on **Forgejo** (self-hosted) for Git hosting and **GitLab Runners** (or Forgejo Actions) for compute.

- **Privacy:** 100% control over our source code and build logs.
- **Resource Isolation:** Dedicated runners for high-performance mobile builds (macOS/ARM) and backend builds (Linux).

## 3. Automatic Deployments (Coolify)

For infrastructure management, we leverage **Coolify** to act as our self-hosted PaaS.

- **Flow:** Forgejo Webhook → Coolify Deployment Hook → Docker Build → Image Push → Service Restart.
- **Auto-SSL:** Handled automatically by the built-in Traefik/Cert-Manager integration.
- **Environment Parity:** Staging and Production clusters are identical in configuration, differing only in resource quotas and data pinning.

## 4. Deployment Strategies

### A) Blue/Green Strategy
For T3 (Core App) and T4 (Worker), we use a Blue/Green approach at the Gateway (T1) layer.

- **Mechanism:** Spin up a new stack ("Green"), verify health, and swap the Nginx/Kong target.
- **Rollback:** Instantaneous swap back to the "Blue" stack if health metrics drop <95%.

### B) Canary Releases
Used for rolling out risky feature changes. 5% of traffic is routed to the new version, monitored in ClickHouse (T7) for exception spikes.

## 5. Mobile OTA (Shorebird)

For the Flutter mobile app, we use **Shorebird.dev** for Over-The-Air (OTA) updates.

- **Bypass:** Allows immediate bug fixes without waiting for App Store/Play Store review cycles.
- **Scope:** Primarily for Dart-level logic changes. Native bridge changes (Kotlin/Swift) still require a new binary build and store submission.

## 6. Implementation Checklist (CI Gate)

A pull request is only "Mergeable" if:
1. `dagger run test` passes (Unit + Integration).
2. `dagger run lint` passes (Go/TS/Flutter).
3. `trivy` scan returns zero `CRITICAL` vulnerabilities.
4. `k6` benchmark is within 5% of the production baseline.

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard*
