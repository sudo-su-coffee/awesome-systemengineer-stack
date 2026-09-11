# 🛡️ Security & Governance Protocol
> **BlackLoverTech Industrial Standard** | April 2026
> [← Master Index](file:///d:/brain/source/Master_Source_Index.md) | Status: **Sovereign**

This document defines the **Shield Layer**—the absolute defensive and regulatory posture of the platform. It codifies Zero-Trust security, multi-jurisdictional compliance, OIDC-based identity, and the deterministic error taxonomy contract.

---

## 🏛️ 1. Zero-Trust Identity & Access (ZIA)

### Infrastructure Security
- **mTLS Everywhere**: Internal service-to-service communication is encrypted and authenticated via mutual TLS (T9).
- **OIDC Gateway**: All user/agent auth is managed via **Zitadel (T2)**. Tokens verified at the Gateway (T1).
- **Rootless Runtimes**: Container processes run as low-privileged system users on Distroless/Alpine.

### Auth Standards (OIDC/JWT)
- **Frontend Flow**: Authorization Code Flow with **PKCE** (Proof Key for Code Exchange). Zero client secrets in mobile/browser.
- **JWT Security**: Signed with `RS256` or `EdDSA`. 15-min access tokens; 30-day rotating refresh tokens (Rotation on every use).
- **Mandatory Claims**: Every JWT must contain `tenant_id` (mandatory for RLS) and `sid` (Session ID for instant server-side revocation).
- **Service Identity**: Internal APIs use Client Credentials Flow with mTLS. Personal Access Tokens (PATs) for system users carry a 90-day TTL.

---

## 🔒 2. Data Sovereignty & Residency

### Jurisdictional Clusters & Residency
- **Physical Pinning**: Data residing in the EU (GDPR) or India (DPDP) is physically locked to isolated clusters in relevant regions. A tenant's `data_residency_region` determines their T6 (Truth) and T10 (Media) physical path.
- **Data Governance**: Zero-Read policy on raw PII in ClickHouse. All logs are obfuscated or pseudonymized at the OTel Collector layer.
- **SOC2 Type II**: Just-in-Time (JIT) authorized access. No platform engineer can access raw tenant data without an authorized request logged in the Audit Tier (T7).

### WAF & Hardened Ingress
- **Edge Layer Protection**: The T1 Gateway (Nginx/Kong) utilizes **ModSecurity/Naxsi** WAF rules to prevent SQLi, XSS, and **BOLA** (Broken Object Level Authorization).
- **Rate Limiting**: Redis-backed limits differentiated by client type:
  - **Public API**: Low-threshold (e.g., 60 RPM).
  - **Internal Agents**: High-threshold (e.g., 5000 RPM).
  - **Webhooks**: Validated against Meta's public IP ranges only.
- **SIEM & Active Defense**: Any cross-tenant access attempt (matching wrong `org_id`) triggers an immediate `ERR_AUTH_BOLA` event, killing the session and logging the incident for security review.

---

## 🔑 4. Industrial Cryptographic Standards

- **Password Hashing**: Argon2id (64MB memory, 3 iterations) + 128-bit Salt + 256-bit Pepper (Vault).
- **Key Rotation**: 90-day cycle via Vault (T9); core app never sees the master Root Key.
- **Asymmetric Ops**: Ed25519 for tokens/signatures; RSA-4096 for hardware terminal handshakes.

---

## 📑 5. The "ERR_" Taxonomy Contract

All system failures are deterministic and follow the `ERR_[DOMAIN]_[REASON]` structure.

### Domain Code Ranges
- `1000–1999`: WhatsApp / Meta
- `2000–2999`: Payments / Hyperswitch
- `3000–3999`: Auth / SSO / Tenancy
- `4000–4999`: DB / Infrastructure
- `9000–9999`: System Health & Security

### Error Response Schema
```json
{
  "success": false,
  "error_code": "ERR_WHATSAPP_RATE_LIMIT",
  "message": "Human readable message",
  "domain": "whatsapp",
  "meta_trace_id": "Required for Meta support",
  "actionable": true
}
```

---
*Created by Antigravity | Defensible-by-Design — BlackLoverTech*
> [↑ Return to Command Center](file:///d:/brain/source/Master_Source_Index.md)
