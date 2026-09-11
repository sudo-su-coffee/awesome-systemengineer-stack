# 🔐 Auth & SSO Engineering Stack

This document codifies the identity and access management (IAM) standards for the Universal SaaS platform, ensuring secure, multi-tenant, and scalable authentication.

## 1. Identity Providers (IdP)

We standardize on **Zitadel** (primary), **Casdoor** (agent-first), and **Keycloak** (enterprise legacy support) as our root-of-trust identity engines.

| Feature | Zitadel (Default) | Casdoor (Alternative) | Keycloak (Fallback) |
|---|---|---|---|
| Level | T2 Identity Tier | Agent-First / IAM | Enterprise / Legacy |
| Runtime | Go | Go | Java / Quarkus |
| Multi-tenancy | First-class "Organizations" | Organizations / Apps | Realms |
| Protocol | OIDC / SAML 2.0 | OIDC / OAuth / SAML | OIDC / SAML 2.0 |
| Use Case | Core SaaS Auth | Agentic / LLM Gateway | Legacy Enterprise |

## 2. OIDC Flows

We enforce strict OpenID Connect (OIDC) compliance for all clients.

- **Frontend (Vue 3/Flutter):** Authorization Code Flow with **PKCE** (Proof Key for Code Exchange). Zero client secrets in the browser/mobile app.
- **Internal APIs:** Client Credentials Flow with mTLS.
- **System Users:** Personal Access Tokens (PATs) with 90-day TTL.

## 3. JWT Lifecycle & Security

JSON Web Tokens (JWT) are the "glue" that propagates identity between the Gateway and internal microservices.

- **Signature Alg:** `RS256` or `EdDSA` (never `HS256`).
- **Rotation:** Automated 30-day key rotation via HashiCorp Vault (T9).
- **Claims Mapping:**
  - `sub`: Unique User ID.
  - `org_id` / `tenant_id`: Mandatory for RLS enforcement.
  - `scope`: Granular permissions (e.g., `whatsapp:send`, `crm:read`).
  - `sid`: Session ID for instant server-side revocation.
- **Lifecycle:**
  - `access_token`: 15-minute TTL.
  - `refresh_token`: 30-day TTL (Rotation on every use).

## 4. Multi-tenant Session Management

Multi-tenancy is enforced at the identity layer, not just the application layer.

- **Isolation:** Every JWT MUST contain a `tenant_id`. The Go backend uses `fastglue` middleware to extract this and inject it into the request context.
- **Organization Switcher:** Handled by Zitadel's "Organizations" feature; switching tenant results in a new session and JWT.
- **Cross-tenant Safety:** RLS (Row Level Security) in PostgreSQL is keyed to the `tenant_id` claim in the JWT.

## 5. Impersonation Rules ("God Mode" for Support)

To debug customer issues, support agents may require impersonation.

- **Trigger:** Only via authenticated Admin Portal action with a logged "Justification".
- **Execution:** Zitadel impersonation API generates a short-lived token with an `impersonator` claim.
- **Audit:** Every impersonated action is logged in Tier 7 (ClickHouse) with both the `actor_id` (agent) and `subject_id` (customer).
- **Restrictions:** Financial actions (refunds, plan changes) require Multi-Factor Authentication (MFA) from the agent even during impersonation.

## 6. Implementation Specifics (Go)

```go
func AuthMiddleware(next fastglue.HandlerFunc) fastglue.HandlerFunc {
    return func(c *fastglue.RequestCtx) error {
        token := c.Request.Header.Peek("Authorization")
        claims, err := ValidateZitadelToken(token)
        if err != nil {
            return c.Status(401).JSON(ErrorMapping(ERR_AUTH_INVALID_TOKEN))
        }
        
        // Pin tenant context early
        c.SetUserValue("tenant_id", claims["tenant_id"])
        c.SetUserValue("user_id", claims["sub"])
        
        return next(c)
    }
}
```

## 7. Secrets & MFA (Sovereign Tools)

To maintain high-agency control over sensitive credentials and second-factor authentication, we utilize the following self-hosted standards:

- **Secrets Management:** **[Infisical](https://github.com/Infisical/infisical)** (T9 Secrets Tier) for platform secrets, certificates, and privileged access management. Replaces/complements HashiCorp Vault with a more developer-friendly UI.
- **MFA Management:** **[2FAuth](https://github.com/Bubka/2FAuth)** for managing Two-Factor Authentication accounts and generating security codes via a web-based dashboard.

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard — BlackLoverTech*
