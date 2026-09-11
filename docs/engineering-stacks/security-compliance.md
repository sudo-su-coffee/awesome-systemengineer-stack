# 🛡️ Universal SaaS: Security & Compliance Stack

This document defines the **Shield Layer** of the platform—synthesizing **Zero-Trust architectures**, **Hardened Container Security**, and **Industrial Compliance (SOC2/ISO)**. It is the blueprint for a "Defensible-by-Design" SaaS infrastructure.

---

## 🏛️ 01. Zero-Trust Identity (mTLS & OIDC)

We assume the internal network is untrusted. Every service must verify the identity of the requester.

- **Mutual TLS (mTLS)**: Internal service-to-service communication (e.g., Gateway to App) is encrypted and authenticated via mTLS. Any service without a valid cert is instantly rejected.
- **OAuth2 / OIDC Integration**: All user and agent authentication is managed via **OpenID Connect (OIDC)**. Tokens are verified at the **API Gateway** layer, ensuring the core `app` only receives pre-authenticated requests.
- **Scoped Permissions**: We utilize **Identity-Based Authorization**. A "Support Agent" token physically cannot trigger a "System Admin" endpoint at the network level.

---

## 📦 02. Hardened Container Security

Security starts at the build layer.

- **Immutable Images**: All production containers use **Alpine** or **Distroless** base images (minimizing the attack surface to ~5MB).
- **Vulnerability Scanning (CI)**: Every Docker image is scanned by **Trivy** or **Grype** during the build. Any image with `HIGH` or `CRITICAL` vulnerabilities results in a failed CI build.
- **Rootless Execution**: No container runs as `root`. Every process is jailed to a low-privileged system user within the container namespace.

---

## 🕸️ 03. WAF & Hardened Ingress (Nginx/Cloudflare)

The only public-facing layer is the **Ingress Gateway**.

- **WAF Rule Sets**: Integration of **OWASP Top 10** protection rules to prevent SQLi, XSS, and BOLA (Broken Object Level Authorization) attacks.
- **Aggressive Rate Limiting**: Redis-backed rate limits differentiated by:
    - **Public API**: Low-threshold (e.g., 60 RPM).
    - **Internal Agents**: High-threshold (e.g., 5000 RPM).
    - **Webhooks**: Validated against Meta's public IP range only.
- **DDoS Mitigation**: Automated "Tarpitting" logic—slowing down suspicious traffic patterns before they hit the application CPU.

---

## 🔒 04. Data Sovereignty & RLS (Zero-Leakage)

We enforce multi-tenant isolation at the storage level.

- **PostgreSQL RLS (Row-Level Security)**: Every financial and messaging table uses **RLS policies** keyed to `tenant_id`. It is mathematically impossible for a query from Tenant A to return data from Tenant B, even in the event of a code-level SQL injection.
- **Encryption at Rest & Transit**: 
    - **Rest**: All DB volumes are encrypted via AES-256. 
    - **Transit**: Mandatory TLS 1.3 for all database connections.

---

## 🔑 06. Industrial Cryptographic Standards

We treat cryptography as a high-precision engineering discipline.

### 1. Password Hashing (The Argon2id Standard)
We reject legacy MD5/SHA-1 and weak Bcrypt implementations.
- **Algorithm**: **Argon2id** (The winner of the Password Hashing Competition).
- **Hardness**: Configured with a minimum of 64MB memory cost and 3 iterations to resist GPU-based brute-force attacks.
- **Salting & Peppering**: 
    - **Salt**: Every password has a unique, random 128-bit salt stored in the DB.
    - **Pepper**: A global, 256-bit secret stored **outside the database** (in Vault). This ensures that even a full DB dump is useless for cracking passwords.

### 2. Asymmetric Cryptography (RSA & Ed25519)
- **Signatures**: We use **Ed25519 (EdDSA)** for all digital signatures (API tokens, Webhook signatures). It is faster and more secure than legacy RSA/ECDSA.
- **Data Exchange**: We support **RSA-4096** for hardware-terminal handshakes and legacy enterprise integrations requires high-bit-depth RSA.

### 3. Cryptographic Lifecycle (Key Rotation)
- **Zero-Static Keys**: All shared secrets are rotated every 90 days.
- **KMS Orchestration**: We utilize a **Key Management Service (KMS)** or HashiCorp Vault to derive ephemeral encryption keys. The `app` never sees the master "Root Key."

---

## 📜 05. Industrial Compliance (SOC2/ISO 27001)

We maintain a continuous audit-ready state.

- **Access Transparency (Audit Logs)**: Every read/write action on sensitive data is logged to a secure, immutable ClickHouse table. 
- **Secret Hygiene**: Zero-Env-Secrets. All production secrets (API keys, DB passwords) are injected as read-only files at `/run/secrets/` by the secret provider (Vault/Docker Secrets).
- **Incident Response Playbook**: Formalized procedures for threat detection, containment, and notification as per GDPR/DPA standards.

## 🔒 07. Active Security & Pen-Testing (Red Team Tools)

To maintain an offensive security posture and ensure the integrity of the sovereign stack, the following "Red Team" and automated auditing tools are utilized:

- **Security Framework:** **[Mantis](https://github.com/PhonePe/mantis)** — A high-performance security framework that automates the workflow of discovery, reconnaissance, and vulnerability scanning.
- **Autonomous Auditing:** **[Hodor](https://github.com/mr-karan/hodor)** — An agentic code reviewer for GitHub PRs and GitLab MRs. Performs multi-step reasoning with autonomous tool orchestration to catch bugs requiring cross-file analysis.
- **Pen-Testing Resources:** **[PHP-Webshells](https://github.com/JohnTroony/php-webshells)** — A curated collection of common PHP webshells used specifically for authorized Penetration Testing and CTF challenges. *Mandatory Rule: Never host these on production servers.*

---
*Last Updated: April 2026 | Following the Universal SaaS Engineering Standard — BlackLoverTech*
