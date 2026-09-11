# 🔒 Auth Protocols Reference

A comprehensive technical reference for modern authentication and authorization protocols, standards, and patterns.

---

## 1. Core Protocols

### OIDC (OpenID Connect)
*Identity layer on top of OAuth2. Issues `id_token` (JWT) with user identity details.*

- **Use for:** User login, Single Sign-On (SSO), pulling user profile data.
- **Key Concepts:** `id_token`, `sub` (identity claim), `userinfo` endpoint.
- **Standard Flow:**
  `App` → `/authorize?scope=openid profile` → `IdP Login` → `code` → `/token` → `id_token + access_token`

> [!NOTE]
> **Wazi Implementation:** Extract `org_id`, `email`, and `sub` from `id_token` claims on first login to bind users to their respective organizations.

### OAuth 2.0
*An authorization framework for delegated access. Issues `access_token` for API access on behalf of a user.*

- **Use for:** Scoped API access, multi-service communication.
- **Grant Types:** 
    - **Authorization Code + PKCE:** Web and Mobile apps.
    - **Client Credentials:** Machine-to-Machine (M2M) / Service-to-Service.
    - **Device Code:** CLIs, IoT, Smart TVs.
    - **Refresh Token:** Silent token renewal.

| Token Type | Purpose | Typical Lifespan |
| :--- | :--- | :--- |
| `access_token` | Short-lived bearer token for APIs | 15 minutes |
| `refresh_token` | Long-lived token for new access tokens | 7–30 days |

---

## 2. Token Standards

### JWT (JSON Web Token - RFC 7519)
*Signed, self-contained token consisting of Header, Payload, and Signature.*

- **Structure:** `header.payload.signature`
- **Security:** RS256 (Public/Private key pair) is standard for OIDC.
- **Common Claims:** `iss` (Issuer), `sub` (User ID), `aud` (Audience), `exp` (Expiry).

> [!WARNING]
> Never put secrets in a JWT payload. It is base64 encoded and publicly readable. Use **JWE** if encryption is required.

### JWKS (JSON Web Key Set - RFC 7517)
*A JSON set of public keys published by the IdP.*

- **Utility:** Allows APIs to validate JWT signatures locally/offline without a round-trip to the IdP for every request.
- **Implementation:** Middleware fetches keys from `/.well-known/jwks.json` and caches them with a TTL.

### PKCE (Proof Key for Code Exchange - RFC 7636)
*Security extension for OAuth2 to prevent auth-code interception.*

- **Flow:**
  1. Generate random `code_verifier`.
  2. Compute `SHA256(code_verifier)` → `code_challenge`.
  3. Send challenge during `/authorize`.
  4. Send raw verifier during `/token` exchange.
- **Mandatory for:** Single Page Applications (SPAs) and Mobile Apps.

---

## 3. Federation & Enterprise SSO

### SAML 2.0 (Security Assertion Markup Language)
*XML-based enterprise SSO protocol. Predates OAuth but remains dominant in large corporate and government sectors.*

- **Mechanism:** Exchange of XML-signed "assertions."
- **Roles:** Service Provider (SP - your app) and Identity Provider (IdP - e.g., Okta, ADFS).
- **Comparison to OIDC:** XML vs. JSON; POST-binding vs. Redirects; No native mobile support.

### SCIM 2.0 (System for Cross-domain Identity Management)
*REST API standard for automated user provisioning and deprovisioning.*

- **Workflow:** 
  `Okta/Azure AD` → `POST /scim/v2/Users` → `Provision User in your DB`
  `Employee Fired` → `DELETE /scim/v2/Users/{id}` → `Instant Session Revocation`

---

## 4. Session & Token Patterns

### Session Cookie Pattern
- **Mechanism:** Server-side state store (e.g., Redis). Browser holds an opaque session ID in an `HttpOnly; Secure; SameSite=Lax` cookie.
- **Benefit:** Instant revocation (delete key in Redis).

### Refresh Token Rotation
- **Mechanism:** Every use of a refresh token issues a *new* refresh token and invalidates the *old* one.
- **Security:** If an attacker and a user both attempt to use the same refresh token, theft is detected and all current sessions are revoked.

### Machine-to-Machine (M2M)
- **Grant:** Client Credentials.
- **Context:** No user profile; Service A authenticates with `client_id` + `client_secret` to call Service B.

---

## 5. Self-Hosted IdP Tools

| Tool | Focus | Technology Stack |
| :--- | :--- | :--- |
| **Keycloak** | De-facto enterprise standard | Java |
| **Zitadel** | Go-based, multi-tenancy native | Go |
| **Authentik** | Lightweight, high customizability | Python/Django |
| **Dex** | Connector model for Kubernetes | Go |

---

> [!IMPORTANT]
> **Wazi Recommendation:** Favor letting organizations bring their own IdP (BYO-IdP). Store the OIDC configuration per organization rather than managing a centralized user database.
