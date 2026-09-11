# 📑 Universal SaaS: Error Taxonomy & Standards Engineering Stack

This document defines the **Standard Engineering Taxonomies** and **Contract-First Error Handling** for the platform.

### The Platform Philosophy: Mechanical Sympathy
Aligned with **Zerodha's "Common Sense Scaling"**, we treat errors as first-class citizens:
1. **Ambiguity is the Enemy**: 400/500 codes are not enough. Every failure must have a deterministic `ERR_` string.
2. **Transparent Failure**: Errors must hint at the remediation strategy (e.g., "Scale up PgBouncer" or "Appeal in Meta Manager").
3. **Fail Securely**: Auth and Tenancy errors (BOLA) must log to SIEM and kill sessions immediately.

---

## 1. Global Error Architecture

### The `ERR_` Taxonomy
We standardize our internal errors using a string-based `ERR_` code system. This replaces ambiguous HTTP status codes or arbitrary numeric IDs. It allows developers (and Support Agents) to immediately identify the domain and failure mode.

**Structure**: `ERR_[DOMAIN]_[SPECIFIC_REASON]`

### The 1000-Code Mapping Convention
To ensure massive scalability (3000+ codes), The Platform reserves code ranges for specific subsystems:
- `1000-1999`: WhatsApp Business Platform (Direct Meta Mapping)
- `2000-2999`: Payments & Hyperswitch Gateway
- `3000-3999`: Auth, SSO & Tenancy
- `4000-4999`: Database, ClickHouse & Infrastructure
- `5000-5999`: Chatbot, AI & Flows
- `6000-6999`: Campaigns & Bulk Marketing
- `7000-7999`: Calling, SIP & IVR
- `8000-8999`: CRM, Contacts & Data ETL
- `9000-9999`: System Health & Internal Security

### The Standard Error Schema (JSON)
All backend API responses (Go/fasthttp) and internal logs (ClickHouse via Vector) MUST use this exact schema when an error occurs:

```json
{
  "success": false,
  "error_code": "ERR_WHATSAPP_RATE_LIMIT",
  "message": "User blocked too many messages; limit reached.",
  "domain": "whatsapp",
  "grpc_status": "RESOURCE_EXHAUSTED",
  "meta_trace_id": "A1b2c3d4e5f6g7h8i9j0", 
  "actionable": false,
  "details": {
    "phone_number_id": "123456789",
    "meta_error_code": 131048
  }
}
```

| Field | Type | Description |
|---|---|---|
| `fbtrace_id` | Meta's internal trace ID. | REQUIRED for Meta escalations. |
| `log_level` | `INFO`, `WARN`, `ERROR`, `CRITICAL` | Routing logic for the Observability Tier. |

### Rule: Code-to-Doc Parity (CI Enforcement)
This document is a **strict engineering contract**.
- **Rule**: Any code passed to `SendErrorEnvelope` in the Go codebase MUST exist in this document.
- **Enforcement**: A CI linter verifies this parity. Undocumented error codes will result in a **Failed Build**.

### Logging & Observability (ClickHouse)
- **Rule 1**: Errors must be logged to ClickHouse (`logchef`) with the `error_code` indexed.
- **Rule 2**: `CRITICAL` tagged errors (e.g. `ERR_AUTH_BOLA`) must trigger immediate Slack/PagerDuty alerts.
- **Rule 3**: If the error originates from a third party (Meta, Stripe), we MUST log their trace ID (e.g., `fbtrace_id`) in our payload. Support cannot escalate to Meta without it.

---

## 2. Global REST & Request Validation (`ERR_REST_`)
Standardized errors for common HTTP request failures across all endpoints.

| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_REST_BODY_MALFORMED`  | `4001` | JSON/TOML request body could not be decoded. | Return `400`. Client must check payload schema. |
| `ERR_REST_UUID_INVALID`    | `4002` | Path or Body UUID parameter is malformed. | Return `400`. Validate ID format on frontend. |
| `ERR_REST_QUERY_INVALID`   | `4003` | URL query parameters (page/limit/sort) are invalid. | Return `400`. Fallback to safe defaults (10/1). |
| `ERR_REST_UNAUTHORIZED`    | `4004` | Request lacks valid authentication credentials. | Return `401`. Redirect to `/auth/login`. |
| `ERR_REST_FORBIDDEN`       | `4005` | Authenticated user lacks permission for the resource. | Return `403`. Show "Access Denied" overlay. |
| `ERR_REST_NOT_FOUND`       | `4006` | The requested route or resource does not exist. | Return `404`. Show 404 illustration. |
| `ERR_REST_METHOD_DENIED`   | `4007` | Endpoint does not support the requested HTTP method. | Return `405 Method Not Allowed`. |
| `ERR_REST_CONFLICT`       | `4008` | Request conflicts with current state (e.g. duplicate key). | Return `409`. Prompt user to refresh or change input. |
| `ERR_REST_RESOURCE_GONE`   | `4009` | Referenced resource (e.g. Org/Account) is deleted. | Return `410 Gone`. Refresh client-side state. |
| `ERR_REST_PAYLOAD_TOO_LARGE`| `4010` | Request body exceeds maximum allowed size (e.g. 10MB). | Return `413`. Client must compress or split data. |
| `ERR_REST_UNSUPPORTED_MEDIA`| `4011` | Content-Type header is missing or unsupported. | Return `415`. Client must use `application/json`. |
| `ERR_REST_UNPROCESSABLE`   | `4012` | Semantic errors in the request body (Validation). | Return `422`. Show inline field-level errors. |
| `ERR_REST_TOO_MANY_REQUESTS`| `4013` | Client has reached the global/per-user rate limit. | Return `429`. Block additional requests for X seconds. |
| `ERR_REST_GATEWAY_TIMEOUT` | `4014` | Downstream service (Redis/PG) failed to respond. | Return `504`. Retry with exponential backoff. |

---

## 3. WhatsApp Business Platform Errors (`ERR_WA_`)

WhatsApp errors are derived directly from Meta's webhook error payloads.

| Internal Code | Meta Code | Name | The Platform Action / Strategy |
|---|---|---|---|
| `ERR_WA_AUTH_EXCEPTION` | `0` | Auth Exception | Regenerate access token. Alert ops. |
| `ERR_WA_API_UNKNOWN` | `1` | API Unknown | Check platform status. Validate request format. Retry with backoff. |
| `ERR_WA_API_SERVICE` | `2` | API Service | Check platform status. Wait and retry. |
| `ERR_WA_API_METHOD` | `3` | API Method | Check token scope. Generate new system user token. |
| `ERR_WA_TOO_MANY_CALLS` | `4` | API Too Many Calls | Implement exponential backoff + jitter. Queue requests. |
| `ERR_WA_PERMISSION_DENIED` | `10` | Permission Denied | Re-grant permissions. Rotate token. |
| `ERR_WA_PHONE_DELETED` | `33` | Parameter Invalid (Phone Deleted) | Verify phone numbers in WABA. Update config. |
| `ERR_WA_INVALID_PARAM` | `100` | Invalid Parameter | Review endpoint reference. Fix parameters. |
| `ERR_WA_TOKEN_EXPIRED` | `190` | Access Token Expired | **CRITICAL**. Alert ops. Renew system user token. Halt queues. |
| `ERR_WA_API_PERMISSION` | `200–299` | API Permission | Debug token scope. Re-grant. |
| `ERR_WA_POLICY_VIOLATION` | `368` | Temporarily Blocked — Policy | Freeze WABA. Send critical alert to tenant to appeal in Business Manager. |
| `ERR_WA_THROUGHPUT_REACHED` | `130429` | Cloud API Throughput Reached | Throttle sends. Use a rate limiter. Request higher throughput if needed. |
| `ERR_WA_USER_IN_EXPERIMENT` | `130472` | User in Experiment | Do not retry. This is expected behavior. |
| `ERR_WA_COUNTRY_RESTRICTION` | `130497` | Country Restriction | Inform tenant of geo-restriction. Do not retry. |
| `ERR_WA_INTERNAL_ERROR` | `131000` | Unknown Internal Error | Retry with exponential backoff. If persistent, capture `fbtrace_id`. |
| `ERR_WA_ACCESS_DENIED` | `131005` | Access Denied | Verify token permissions. Generate new token. |
| `ERR_WA_REQUIRED_PARAM_MISSING` | `131008` | Required Parameter Missing | Check endpoint reference. Add missing field. |
| `ERR_WA_PARAM_VALUE_INVALID` | `131009` | Parameter Value Invalid | Validate inputs before sending. Check formats. |
| `ERR_WA_SERVICE_UNAVAILABLE` | `131016` | Service Unavailable | Check platform status. Retry. |
| `ERR_WA_RECIPIENT_CANNOT_BE_SENDER` | `131021` | Recipient Cannot Be Sender | Routing bug in code. Fix recipient resolution. |
| `ERR_WA_MESSAGE_UNDELIVERABLE` | `131026` | Message Undeliverable | Mark contact as potentially invalid. Do not spam-retry. |
| `ERR_WA_ACCOUNT_LOCKED` | `131031` | Account Locked | Diagnose via Business Support Home. Reset PIN / Appeal if policy. |
| `ERR_WA_DISPLAY_NAME_APPROVAL_NEEDED` | `131037` | Display Name Approval | Complete display name approval in WhatsApp Manager. |
| `ERR_WA_BUSINESS_ELIGIBILITY_ISSUE` | `131042` | Business Eligibility Issue | Check Meta payment method. Resolve billing issue. |
| `ERR_WA_INCORRECT_CERTIFICATE` | `131045` | Incorrect Certificate | Re-register the phone number with the correct certificate. |
| `ERR_WA_OUT_OF_WINDOW` | `131047` | Re-engagement Message | Drop message. UI should prompt user to send Template instead. |
| `ERR_WA_SPAM_BLOCK` | `131048` | Spam Rate Limit Hit | **Do NOT retry.** Alert tenant that account rating has dropped. |
| `ERR_WA_META_CHOSE_NOT_TO_DELIVER` | `131049` | Meta Chose Not to Deliver | Do not retry. Review content and sending patterns. |
| `ERR_WA_UNSUPPORTED_MESSAGE_TYPE` | `131051` | Unsupported Message Type | Log and surface as "Unsupported" in UI. No retry needed. |
| `ERR_WA_MEDIA_DOWNLOAD_ERROR` | `131052` | Media Download Error | Re-fetch media URL. Implement retry with re-fetch logic. |
| `ERR_WA_MEDIA_UPLOAD_ERROR` | `131053` | Media Upload Error | Retry upload. Check file format and size limits. |
| `ERR_WA_PAIR_RATE_LIMIT` | `131056` | Pair Rate Limit Hit | Do not retry to that recipient. Continue sending to others. |
| `ERR_WA_ACCOUNT_IN_MAINTENANCE` | `131057` | Account in Maintenance | Wait. Retry after a few minutes. Alert tenant. |
| `ERR_WA_UNSUPPORTED_MSG_TYPE_COEXISTENCE` | `131060` | Coexistence Msg Type | Wait 5 seconds and retry once. |
| `ERR_WA_TEMPLATE_MISMATCH` | `132000` | Template Param Count Mismatch | Fix variable count. Sync template definitions. |
| `ERR_WA_TEMPLATE_DOES_NOT_EXIST` | `132001` | Template Does Not Exist | Verify template status. Sync approved templates. |
| `ERR_WA_TEMPLATE_HYDRATED_TEXT_TOO_LONG` | `132005` | Text Too Long | Truncate variable values before sending. |
| `ERR_WA_TEMPLATE_FORMAT_POLICY` | `132007` | Format Policy Violated | Review and re-submit template for approval. |
| `ERR_WA_TEMPLATE_PARAM_FORMAT_MISMATCH` | `132012` | Param Format Mismatch | Fix parameter types (e.g., text instead of currency). |
| `ERR_WA_TEMPLATE_PAUSED` | `132015` | Template Paused (Quality) | Stop sending. Auto-fallback to alternative if configured. |
| `ERR_WA_TEMPLATE_DISABLED` | `132016` | Template Disabled | Stop sending. Create new compliant template. Alert tenant. |
| `ERR_WA_FLOW_BLOCKED` | `132068` | Flow Blocked | Check Flow status. Do not send until unblocked. |
| `ERR_WA_FLOW_THROTTLED` | `132069` | Flow Throttled | Retry with backoff. |
| `ERR_WA_INCOMPLETE_DEREGISTRATION` | `133000` | Incomplete DeregISTRATION | Complete deregistration flow first. |
| `ERR_WA_SERVER_TEMPORARILY_UNAVAILABLE` | `133004` | Registration Unavailable | Retry after a few minutes. |
| `ERR_WA_2STEP_PIN_MISMATCH` | `133005` | 2-Step PIN Mismatch | Prompt user for correct PIN. |
| `ERR_WA_PHONE_REVERIFICATION_NEEDED` | `133006` | Phone Re-verification Needed | Re-run verification flow (`request_code` -> `verify_code`). |
| `ERR_WA_TOO_MANY_PIN_GUESSES` | `133008` | Too Many PIN Guesses | Wait for lockout to expire. |
| `ERR_WA_PIN_GUESSED_TOO_FAST` | `133009` | PIN Guessed Too Fast | Add delay between PIN attempts. |
| `ERR_WA_PHONE_NOT_REGISTERED` | `133010` | Phone Not Registered | Register the number before sending. |
| `ERR_WA_PLEASE_WAIT_BEFORE_REREGISTERING`| `133015` | Wait Before Re-registering | Wait before retrying registration. |
| `ERR_WA_REG_RATE_LIMIT` | `133016` | Registration Limit Exceeded | Wait 72 hours. Block UI retry buttons. |
| `ERR_WA_PAYMENTS_TERMS_NOT_ACCEPTED`| `134011` | Payments Terms Not Accepted | Tenant must accept terms in WhatsApp Manager. |
| `ERR_WA_GENERIC_USER_ERROR` | `135000` | Generic User Error | Check `error_data.details` for specifics. Log and retry if applicable. |
| `ERR_WA_ACCOUNT_DISCONNECTED`| `135001` | WABA is disconnected or has unaccepted terms. | Redirect to Meta Business Manager for re-verification. |

---

## 3. Mobile & Edge Gateway Errors (`ERR_MOBILE_`)
Errors originating from the Flutter/Rust client layer or the mobile-specific API bridge.

| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_MOBILE_BRIDGE_PANIC` | `1201` | Rust <-> Dart FFI bridge panic/crash. | **CRITICAL**. Kill app process. Send crash log to Sentry. |
| `ERR_MOBILE_SYNC_STALE`   | `1202` | Local SQlite data is out of sync with backend (>24h). | Trigger full re-sync. Clear local cache. |
| `ERR_MOBILE_OFFLINE_LIMIT`| `1203` | User attempted too many outbound actions while offline. | Buffer actions. Block new additions until sync. |
| `ERR_MOBILE_FINGERPRINT`  | `1204` | Biometric / PIN authentication failed on device. | Lock app. Prompt for manual password fallback. |

---

## 4. Payments, Billing & Subscription Errors (`ERR_PAY_`)

Handled via the Hyperswitch orchestration layer and Lago metering.

| Internal Code | gRPC Status | Name | The Platform Action / Strategy |
|---|---|---|---|
| `ERR_PAY_INSUFFICIENT_FUNDS` | `PAYMENT_REQUIRED` | Insufficient Credits / Funds | Trigger Dunning State Machine (KillBill). Soft warning on UI. |
| `ERR_PAY_ACCOUNT_FROZEN` | `PERMISSION_DENIED` | Account Frozen (Unpaid) | Block `POST /messages`. Return `ERR_PAY_ACCOUNT_FROZEN` gracefully. |
| `ERR_PAY_GATEWAY_TIMEOUT` | `DEADLINE_EXCEEDED` | Upstream Gateway Timeout | Auto-failover via Hyperswitch (e.g., Razorpay -> Cashfree). |
| `ERR_PAY_FRAUD_BLOCK` | `FAILED_PRECONDITION` | Fraud Detection Triggered | Freeze transaction. Flag for manual admin review. |
| `ERR_PAY_HMAC_MISMATCH` | `UNAUTHENTICATED` | Webhook Signature Invalid | **SECURITY**. Reject webhook. Log incoming IP for potential spoofing attack. |
| `ERR_PAY_IDEMPOTENCY_HIT` | `ALREADY_EXISTS` | Duplicate Charge Attempt | Halt charge. Return cached successful transaction receipt. |

*Note: In the event of an `ERR_PAY_HMAC_MISMATCH`, the transaction MUST be treated as hostile.*

---

## 4. Database & Infrastructure Errors (`ERR_DB_`)

PostgreSQL, ClickHouse, and ObjectStore (Blobasaur/S3) standard errors.

| Internal Code | Name | The Platform Action / Strategy |
|---|---|---|
| `ERR_DB_DEADLOCK` | Postgres Transaction Deadlock | Transaction aborted. Retry transaction automatically (max 3 times). |
| `ERR_DB_CONNECTION_LIMIT` | PgBouncer Pool Exhausted | Scale up read replicas. Trigger infrastructure alert. |
| `ERR_DB_WAL_FULL` | Write-Ahead Log Full | Alert DevOps. Temporarily buffer non-critical writes in NATS/Kafka. |
| `ERR_DB_S3_THROTTLED` | S3 Prefix 503 Slow Down | Implement `{0-9}-resource` prefix partitioning logic. Exponential backoff. |
| `ERR_DB_CACHE_MISS` | Redis/Memory Cache Miss | Non-fatal. Fallback to reading from PostgreSQL. |

---

## 5. Mobile & API Gateway Errors (`ERR_CLIENT_`)

Errors related to the Flutter mobile application, gRPC bridges, and auth.

| Internal Code | Description | Client UX Resolution |
|---|---|---|
| `ERR_CLIENT_OFFLINE_SYNC` | Isar DB to Backend sync failed. | Keep data in offline queue. Show amber "Syncing Paused" pill in bottombar. |
| `ERR_CLIENT_SESSION_EXPIRED` | JWT / OIDC token lifetime over. | Force silent token refresh. If refresh fails, log user out immediately. |
| `ERR_CLIENT_BRIDGE_PANIC` | Rust/Dart FFI Thread panic. | Catch exception via Sentry. Show "Severe Client Error" and restart specific UI context. |
| `ERR_CLIENT_SSL_PIN_FAIL` | MITM / Invalid cert detected. | **SECURITY**. Hard kill network layer. Inform user of potential network interception. |

---

## 6. The Platform Platform Core & Action Errors (`ERR_PLATFORM_`)

Internal system errors reflecting proprietary SaaS logic spanning Contact Management (CRM), Template Caching, Media Handling, Agent Assignment, and Real-time Infrastructure.

### Contact Management (CRM)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_CONTACT_STORE_FAILED` | Failed to safely store or update a unified contact in PostgreSQL. | Retry transaction. Show "Contact Sync Failed" UI alert. |
| `ERR_PLATFORM_CONTACT_NOT_FOUND` | Queried contact `BSUID` or Phone ID does not exist in tenant DB. | Return `404`. Prompt agent to create a new profile. |
| `ERR_PLATFORM_CONTACT_DUPLICATE`| A contact with this phone number already exists under the tenant. | Return `409 Conflict`. Offer UI option to Merge contacts. |
| `ERR_PLATFORM_CONTACT_MERGE_ERR`| Failed to merge chat history of two duplicate contacts. | Abort transaction. Keep both contacts independent. |

### Template Engine (templateutil)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_TEMPLATE_SYNC_FAIL`| Unable to retrieve the active marketing template list from Meta. | Serve currently cached templates. Show "Offline Mode" warning. |
| `ERR_PLATFORM_TEMPLATE_INVALID`| Template creation request failed local regex/format validation. | Block API call. Return actionable UI error regarding variable formatting. |
| `ERR_PLATFORM_TEMPLATE_NO_CACHE` | Template not found in local Redis cache during message send. | Fallback to synchronous DB fetch. |
| `ERR_PLATFORM_TEMPLATE_LIMIT`| Organization has reached its internal limit for template storage. | Prompt tenant to upgrade plan or delete unused templates. |

### Agent Assignment & Routing (assignment)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_ASSIGN_NO_AGENT` | Failed to auto-route chat; no agents match the routing rules. | Fallback: Place chat in the global "Unassigned" inbox. |
| `ERR_PLATFORM_ASSIGN_CAPACITY` | All matched agents are at their maximum concurrent chat capacity. | Queue chat in waiting line. Notify supervisors if queue > threshold. |
| `ERR_PLATFORM_ASSIGN_TIMEOUT` | Agent did not accept the routed chat within the SLA window. | Re-route to next available agent or trigger Auto-Responder. |

### Media & Storage (storage)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_MEDIA_TRANSCODE` | Rust transcode process failed or file size exceeds Meta limit. | Reject upload locally. Prompt user to compress media. |
| `ERR_PLATFORM_MEDIA_S3_UPLOAD` | Timeout/Failure while pushing media binary to S3/MinIO. | Retry upload with backoff. Alert Ops if persistent. |
| `ERR_PLATFORM_MEDIA_UNSUPPORTED`| Attempted to upload/send a MIME type not supported by The Platform/Meta. | Reject upload. Show supported formats in UI. |
| `ERR_PLATFORM_MEDIA_NOT_FOUND` | Requested media asset is missing from local ObjectStore/CDN. | Return `404`. Show broken-media placeholder in UI. |

### Authentication, Security & Tenancy (middleware, crypto)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_AUTH_INVALID_TENANT`| Cross-tenant unauthorized access attempt (wrong `org_id`). | **SECURITY**. Hard reject (`403`). Log incident to SOC / Sentry. |
| `ERR_PLATFORM_AUTH_RBAC_DENIED` | User role lacks permission for the requested action (e.g. view billing). | Return `403`. Hide UI buttons requiring this permission. |
| `ERR_PLATFORM_AUTH_ORG_SWITCH`  | User attempted to switch to an organization they do not have membership in. | **SECURITY**. Kill session and return to login. |
| `ERR_PLATFORM_AUTH_ACCOUNT_LOCK`| Organization account has been suspended by administrators. | Prevent login. Display "Account Suspended" support modal. |
| `ERR_PLATFORM_AUTH_USER_RESTRICT`| The specific user has been restricted from accessing the org features. | Display restricted access banner. Contact org Admin. |
| `ERR_PLATFORM_AUTH_API_EXPIRED` | The internal programmatic API Key utilized has expired. | Return `401`. Require admin to generate a new key. |
| `ERR_PLATFORM_CRYPTO_DECRYPT_FAIL`| Failed to decrypt secure payload (e.g., stored credentials/tokens). | **CRITICAL**. Fail securely. Alert Ops on potential master-key rotation issue. |

### Billing & Metering (Lago Integration)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_BILLING_METER_FAIL` | Failed to increment usage meter in Lago (e.g. tracking a sent message). | Queue metric event in Kafka/NATS for async retry. Do not block user. |
| `ERR_PLATFORM_BILLING_USAGE_LIMIT`| Organization has crossed their hard API/message usage limit. | Hard block outbound API. Show "Quota Exceeded" banner. |
| `ERR_PLATFORM_BILLING_TIER_BLOCK` | Feature requested is not available on the current SaaS subscription tier. | Feature gate hit. Prompt user with subscription upgrade modal. |
| `ERR_PLATFORM_BILLING_OVERDUE` | Unpaid Lago invoice past grace period restricting write operations. | Enter "Read-Only" mode. Render global warning header. |

| `ERR_AUTH_SSO_INVALID_DOMAIN`| `3004` | SSO attempted from a blacklisted/unauthorized domain. | Return `403`. Restrict to organization-approved domains. |

### Email Identity & Login Lifecycle
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_AUTH_EMAIL_TAKEN`      | `3101` | Email already exists during registration or SSO link. | Return `409 Conflict`. Suggest password recovery or SSO link. |
| `ERR_AUTH_EMAIL_INVALID`    | `3102` | Email format validation failed or domain blocked. | Return `400`. Prompt for valid corporate email. |
| `ERR_AUTH_CREDENTIALS`     | `3103` | Invalid email or password combination. | Return `401`. Run timing-safe dummy hash comparison. |
| `ERR_AUTH_ACCOUNT_DISABLED` | `3104` | User account has been manually disabled by Admin. | Return `401`. Show "Account Disabled" alert. |
| `ERR_AUTH_PASSWORD_WEAK`    | `3105` | Provided password does not meet complexity requirements. | Return `400`. Show password strength guidelines. |
| `ERR_AUTH_APIKEY_INVALID`   | `3106` | API Key is malformed, revoked, or non-existent. | Return `401`. Do not allow further programmatic hits. |
| `ERR_AUTH_USER_NOT_FOUND`   | `3107` | Email address does not match any registered user. | Return `401`. (Avoid specific 404 to prevent enumeration). |
| `ERR_AUTH_SIGNUP_CLOSED`    | `3108` | Public registration is disabled by the instance admin. | Return `403`. Show "Contact Sales" or "Closed" message. |
| `ERR_AUTH_REG_DOMAIN_DENY`  | `3109` | Email domain is not in the organization's allowed list. | Return `403`. Restrict to corporate email domains. |

### User Sessions & JWT Lifecycle
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_AUTH_SESSION_EXPIRED`  | `3201` | JWT Access Token has expired. | Return `401`. Trigger transparent Refresh Token flow in UI. |
| `ERR_AUTH_SESSION_REVOKED`  | `3202` | Refresh Token was consumed, revoked, or hijacked. | Kill frontend session. Redirect to login `/auth/login`. |
| `ERR_AUTH_SESSION_INVALID`  | `3203` | JWT signature is invalid or tampered with. | **SECURITY**. Log incident. Clear all local cookies. |
| `ERR_AUTH_MFA_REQUIRED`     | `3204` | Account requires 2FA/MFA verification. | Redirect to `/auth/mfa`. Block all other API calls. |

### Organization, Role & Membership (ERR_ORG_)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_ORG_NOT_FOUND`         | `3301` | Requested organization ID does not exist in the tenant DB. | Return `404`. Redirect to global dashboard. |
| `ERR_ORG_MEMBER_ALREADY`   | `3302` | User is already a member of the target organization. | Return `409`. Stop redundant invitation flow. |
| `ERR_ORG_MEMBER_NOT_FOUND` | `3303` | Target user is not a member of the current organization. | Return `404`. Deny organization-specific actions. |
| `ERR_ORG_MEMBER_REMOVE_SELF`| `3304` | Owner attempted to remove themselves from the only org. | Block action. Prompt to transfer ownership first. |
| `ERR_ORG_NAME_REQUIRED`    | `3305` | Organization name field is empty during creation. | Return `400`. Validation failed. |
| `ERR_ORG_ROLE_UNAUTHORIZED` | `3306` | User lacks sufficient role level to manage memberships. | Return `403`. Log permission violation. |

### Application CRUD & Access Control (BOLA/IDOR)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_CRUD_BOLA` | Request `org_id` token does not match the requested DB object `org_id` (IDOR/BOLA attempt). | **SECURITY**. Kill request. Log injection attempt to SIEM. |
| `ERR_PLATFORM_CRUD_INVALID_ID` | Object identifier provided is malformed (e.g., failed UUID parsing). | Return `400`. Require correct format in payload. |
| `ERR_PLATFORM_CRUD_NOT_FOUND` | Queried object (flow, key, role, etc) does not exist in the database. | Return `404`. Show "Item Not Found" in UI. |
| `ERR_PLATFORM_CRUD_VALIDATE` | Payload body failed struct validation inject/schema rules. | Return `400`. List specific field validation failures. |
| `ERR_PLATFORM_CRUD_DEPENDENCY` | Attempt to delete an entity that has active constraints (e.g., deleting a Role still attached to Users). | Return `409 Conflict`. Prompt user to re-assign relations first. |

### Infrastructure (queue, websocket, audit)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_QUEUE_DEAD_LETTER` | Job (e.g. webhook processing) failed max retries and moved to DLQ. | Trigger Admin notification. Leave in DLQ for manual inspection. |
| `ERR_PLATFORM_WS_DROPPED` | Real-time UI websocket disconnected abruptly. | Client must enter reconnect loop. Sync missing events via HTTP `/sync`. |
| `ERR_PLATFORM_WS_AUTH_FAIL` | WebSocket token (`Subject: ws`) is invalid or expired. | Close connection with `4001` code. Re-fetch token. |
| `ERR_PLATFORM_AUDIT_APPEND_FAIL` | Failed to write trace to immutable audit log. | **SECURITY**. Block the mutating request until Audit log recovers. |

### Chatbot Sessions & Conversation State
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_BOT_SESSION_EXPIRED`   | `5501` | Chatbot session timed out (default 30m idle). | Close session. Force start from "Welcome" node on next hit. |
| `ERR_BOT_SESSION_CANCELLED` | `5502` | User manually cancelled the active bot flow. | Stop flow execution. Reset state variables. |
| `ERR_BOT_SESSION_DATA_LOST` | `5503` | Redis persistence for session JSONB data failed. | Recover from DB. If failed, reset to start of block. |
| `ERR_PLATFORM_WEBHOOK_PARSE` | Failed to Unmarshal incoming Meta JSON webhook payload. | Log raw payload to DLQ. Still return `200 OK` to Meta to prevent retry storms. |
| `ERR_PLATFORM_CALLING_SIP_FAIL` | SIP trunk integration failed during a Voice/Calling flow. | Fallback to text message or drop call with standard busy signal. |

### Inbound Meta Webhooks & Message Edge Cases
| Internal Code | Description | UI Display Strategy |
|---|---|---|
| `ERR_PLATFORM_WH_SIGNATURE` | HMAC `X-Hub-Signature` from Meta is missing or invalid. | **Hidden**. Reject request `401`. Log suspected spoofing attack to SIEM. |
| `ERR_PLATFORM_WH_UNSUPPORTED`| Message type (e.g. unknown new Meta Interactive feature) not fully supported by our renderer. | **Screen**. Display inline chat bubble: *"This message type is currently unsupported."* |
| `ERR_PLATFORM_WH_MEDIA_FAIL` | Meta webhook arrived but the attached media pointer has expired/failed to download. | **Screen**. Display inline image placeholder with reload icon. |
| `ERR_PLATFORM_WH_STALE_EVENT`| Webhook delivery was delayed by Meta and timestamp is out of sync. | **Hidden**. Write to DB silently. Re-order chat timeline without pushing a new notification. |
| `ERR_PLATFORM_WH_SYNC_DELAY` | Massive queue backpressure causing webhook processing lag. | **Popup**. Show Amber Toast at top of UI: *"Message sync delayed due to high volume."* |

### Data Processing, Import & ETL (ERR_DATA_)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_DATA_MAP_MISMATCH` | `8001` | CSV headers do not match expected system mapping. | Return `400`. Show "Header Mapping Wizard" to the user. |
| `ERR_DATA_LIMIT_EXCEEDED`| `8002` | Import row count exceeds organization tier limit. | Truncate and process allowed rows; notify of limit. |
| `ERR_DATA_TRANSFORM_FAIL`| `8003` | Failed to convert field (e.g. bad date format in CSV). | Surface row-level error log. Skip row or stop import. |
| `ERR_DATA_PROCESS_TIMEOUT`| `8004` | Large file processing exceeded worker timeout (30s). | Move to background task. Notify via WebSocket when done. |

### Core Messaging & Engagement Engine (Internal)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_MSG_DUPLICATE_UUID` | `1501` | Client-side UUID already exists in DB (Idempotency check). | Ignore send request. Return cached success status. |
| `ERR_MSG_ATTACH_NOT_FOUND`| `1502` | Referenced S3 object for attachment was deleted or 404. | Show inline error in chat: "Media file missing". |
| `ERR_MSG_RENDER_FAIL`    | `1503` | Failed to hydrate template with provided variables. | Return `422`. Highlight missing variables in API response. |
| `ERR_MSG_SLA_BREACH`     | `1504` | Message was not sent within the required internal SLA time. | Escalate via NATS. Alert manager dashboard. |

### System Health, Stability & Security (ERR_SYS_)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_SYS_MAINTENANCE`   | `9001` | Platform is in scheduled maintenance / Read-only mode. | Return `503`. Show "Maintenance" splash screen. |
| `ERR_SYS_SSRF_PROBE`    | `9002` | Request attempted to access internal network (SSRF protection). | **SECURITY**. Hard block. Log IP to firewall blacklist. |
| `ERR_SYS_MEMORY_PRESSURE`| `9003` | Go runtime memory pressure / OOM risk (Circuit Breaker). | Shed load. Drop non-critical background jobs. |
| `ERR_SYS_GRACEFUL_HALT` | `9004` | Server is shutting down. Connection refused. | Client should retry with another node. |

### Agent, Team & Assignment (ERR_TEAM_)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_TEAM_NO_ONLINE_AGENT`| `1701` | Assignment requested but all agents are "Offline" or "Away". | Route to fallback IVR or send auto-responder. |
| `ERR_TEAM_MAX_CAPACITY`  | `1702` | All agents have reached their maximum chat limit. | Queue the contact. Show "Current Queue Position" to user. |
| `ERR_TEAM_AGENT_INACTIVE`| `1703` | Target agent session has expired or they logged out recently. | Re-run assignment logic for next available agent. |
| `ERR_TEAM_AGENT_BUSY`    | `1704` | Agent is online but has manually set state to "Busy/DND". | Skip agent in round-robin assignment. |

### Custom Actions & Webhook Integration
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_ACTION_EXEC_FAIL`  | `9501` | Custom code execution failed (JS/Go script error). | Log error. Return `424 Failed Dependency`. Show logs to admin. |
| `ERR_ACTION_TIMEOUT`    | `9502` | Webhook target did not respond within 10s. | Failure. Stop execution flow. |
| `ERR_ACTION_MALFORMED`  | `9503` | Webhook response payload does not match expected schema. | Ignore response. Notify admin of integration sync issue. |

### Bulk Campaigns & Marketing
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_CAMPAIGN_START_FAIL` | Failed to transition campaign to Running state. | Keep in Draft state. Alert user to check template approval. |
| `ERR_PLATFORM_CAMPAIGN_INVALID_STATE`| Campaign cannot be started/updated in its current lifecycle state. | Return `400`. Refer to State Machine docs. |
| `ERR_PLATFORM_CAMPAIGN_NO_RECIPIENTS`| Attempted to start a campaign with 0 valid recipients. | Block start. Prompt user to upload CSV or select Contacts grid. |
| `ERR_PLATFORM_CAMPAIGN_MEDIA_MISSING`| Attached media for campaign was deleted before campaign execution. | Abort campaign start. Show "Media Missing" validation error. |
| `ERR_PLATFORM_CAMPAIGN_CSV_PARSE` | Failed to parse uploaded recipient CSV file. | Return `400`. Show specific row/column parsing failure. |
| `ERR_PLATFORM_CAMPAIGN_RATE_THROTTLE`| Meta API rate limited our bulk worker. | Transparently pause campaign internally with exponential backoff. |
| `ERR_PLATFORM_CAMPAIGN_DELETE_ACTIVE`| Attempted to delete a campaign that is currently `Processing`. | Block action. Pause/Cancel the campaign first. |

### Chatbot, AI Context & Keyword Rules
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_BOT_KEYWORD_CONFLICT` | Created keyword rule conflicts with an existing exact match. | Reject creation. Show conflicting rule ID to user. |
| `ERR_PLATFORM_BOT_FLOW_PARSE`   | Chatbot Flow node graph failed structural validation. | Return `400`. Highlight orphaned/invalid block in Flow Builder. |
| `ERR_PLATFORM_BOT_AI_TIMEOUT`   | External LLM (OpenAI/Claude) timed out during context generation. | Fallback to next deterministic node or trigger Human Handoff. |
| `ERR_PLATFORM_BOT_TRANSFER_FAIL`| Request to transfer bot session to live agent failed. | Retry assignment. If fails, send offline message to customer. |
| `ERR_PLATFORM_BOT_MAX_TURNS`    | Chatbot reached maximum infinite loop safety threshold. | Kill bot session to save resources. Hand off to agent. |

### IVR Flows & Advanced Calling
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_IVR_AUDIO_UPLOAD` | Intended IVR menu audio file exceeds length limit or bad format. | Reject upload. Prompt for standard MP3/WAV format. |
| `ERR_PLATFORM_IVR_FLOW_INVALID` | DTMF graph tree contains unreachable menu endpoints. | Block save. Highlight unreachable nodes for the admin. |
| `ERR_PLATFORM_CALL_RECORDING_FAIL`| S3 client failed to pipe active SIP audio stream. | Call proceeds, but mark log as "Recording Failed". Alert Ops. |
| `ERR_PLATFORM_CALL_HANGUP_ERR`  | Disconnect signal failed or trunk connection hung. | Force drop ICE session. Reap zombie channel on worker node. |
| `ERR_PLATFORM_CALL_NO_AGENTS`   | Inbound IVR routed to queue with no available SIP agents. | Play offline greeting audio and send missed call WhatsApp template. |

### Commerce Catalogs & Products
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_CATALOG_SYNC_ERR` | Failed to synchronize Meta Commerce Manager catalog. | Show last successful sync time. Provide manual "Force Sync" button. |
| `ERR_PLATFORM_COMM_META_FAIL`    | Meta API rejected product/catalog creation (e.g. Policy violation). | Return `422`. Show Meta reason (e.g. "Prohibited Item"). |
| `ERR_PLATFORM_PRODUCT_NOT_FOUND`| User attempted to send an interactive message with deleted product. | Validate payload. Strip missing products or reject payload. |
| `ERR_PLATFORM_PRODUCT_INVALID`  | Product name, price, or SKU missing during creation. | Return `400`. Highlight missing fields. |
| `ERR_PLATFORM_CATALOG_LIMIT`    | Max synced catalogues per organization reached. | Return `403 Tier Limit`. Prompt upgrade. |

### Analytics & Custom Widgets
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_WIDGET_QUERY_TIMEOUT`| ClickHouse analytical query exceeded 10-second max limit. | Cancel query. Render Widget as "Timeout - Filter date range". |
| `ERR_PLATFORM_WIDGET_DATA_SOURCE` | Invalid metrics dimensions requested by frontend. | Render empty widget placeholder. Recalculate layout state. |
| `ERR_PLATFORM_ANALYTICS_META_CACHE`| Failed to refresh Meta Analytics metrics cache. | Serve stale metrics. Show "Metrics delayed" tooltip in Dashboard. |
| `ERR_PLATFORM_WIDGET_NOT_FOUND` | User attempted to access or delete a non-existent widget. | Return `404`. Redirect to Dashboard overview. |
| `ERR_PLATFORM_WIDGET_LAYOUT_ERR` | Failed to save grid layout positions in DB/Redis. | Show "Layout Save Failed" toast. Retry automatically. |
| `ERR_PLATFORM_WIDGET_OWNER_DENY` | Non-owner attempted to edit/delete a private widget. | Return `403`. Block action. |

### Advanced Metrics & Telemetry (ERR_METRIC_)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_METRIC_AGGREGATION_FAIL` | `4101` | Failed to aggregate time-series data in ClickHouse. | Show "Metrics unavailable" in widget. |
| `ERR_METRIC_DATA_STALE`       | `4102` | Metrics haven't refreshed in >60 minutes. | Show warning sign on Dashboard. Trigger background sync. |
| `ERR_METRIC_DIMENSION_DENY`   | `4103` | Requested dimension is too high-cardinality for the UI. | Suggest filtering by Date or Organization. |
| `ERR_METRIC_SOURCE_DOWN`      | `4104` | Downstream metrics collector (ClickHouse/Redis) is offline. | Show "System Metrics Delayed" global banner. |

### System Heartbeat & Readiness (ERR_SYS_READY_)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_SYS_READY_DB_FAIL`     | `9201` | Server is up but Database connection is dropped. | `/ready` fails (503). Load balancer should remove node. |
| `ERR_SYS_READY_REDIS_FAIL`  | `9202` | Server is up but Redis connection is dropped. | `/ready` fails (503). Block auth/locking dependent jobs. |
| `ERR_SYS_READY_WA_DOWN`     | `9203` | Meta Graph API is experiencing global outage (check status). | Inform Admin. Pause all outbound worker queues. |

### Privacy, GDPR, & Data Retention
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_PRIVACY_DNT_ACTIVE`| Contact has invoked "Do Not Track" / GDPR erasure. Request blocked. | **COMPLIANCE**. Soft fail. Log the suppression and drop request. |
| `ERR_PLATFORM_PRIVACY_RETENTION` | Requested chat history is beyond the tenant's retention policy (e.g., >365 days). | Return `404`. Provide UI tooltip explaining retention rules. |
| `ERR_PLATFORM_PRIVACY_EXPORT_LIMIT`| Tenant exceeded monthly GDPR data export limits. | Delay export via queue. |

### Deep Rate Limiting & Overload Protection (SLA)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_RL_GLOBAL_MAX`    | IP / Tenant breached hard global API rate limit (e.g., 5000/sec). | Immediate `429 Too Many Requests`. Temporarily blacklist IP at load balancer. |
| `ERR_PLATFORM_RL_CONCURRENCY`   | User executed too many concurrent heavy writes. | Fail fast with `429`. UI must implement exponential backoff. |

### Global Storage & S3
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_STORAGE_MAX_SIZE` | Provided chat attachment exceeds 16MB file limit. | Reject `413 Payload Too Large`. Prompt user to compress payload. |
| `ERR_PLATFORM_STORAGE_MIME_DENY`| Attempted to upload blacklisted executable MIME type in chat. | Reject `415`. Raise potential malware alert to SOC. |

### Advanced AI & Local LLM (Piper TTS)
| Internal Code | Description | The Platform Action / Strategy |
|---|---|---|
| `ERR_PLATFORM_AI_CTX_EXCEEDED`  | Passed RAG context is too large for the configured LLM token window. | Truncate oldest context automatically before generating AI response. |
| `ERR_PLATFORM_AI_VOICE_MISSING` | Piper TTS requested a voice model that is not installed on disk. | Fallback to default generic acoustic model. Alert DevOps. |

### WebRTC, SIP & Voice Signalling (ERR_VOICE_)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_VOICE_ICE_STUN_FAIL` | `7501` | Failed to gather ICE candidates from STUN/TURN servers. | User likely behind restrictive firewall. Suggest VPN/TCP fallback. |
| `ERR_VOICE_SIGNAL_TIMEOUT`| `7502` | WebSocket SDP exchange timed out during call setup. | Drop call. Show "Connection Unstable" in UI. |
| `ERR_VOICE_TRUNK_BUSY`   | `7503` | External SIP trunk gateway returned 486 Busy. | Play busy tone. Log as missed call with reason. |
| `ERR_VOICE_PERM_DENIED`  | `7504` | Contact has revoked "Voice Call" permission flag. | Block outgoing call. Show permission request modal. |

### Catalog Management & Inventory (ERR_COMMERCE_)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_COMM_INV_OUT_OF_STOCK`| `8501` | Product requested in cart is marked as 0 inventory. | Return `409`. Stop checkout flow. Notify customer. |
| `ERR_COMM_PAY_METHOD_DENY` | `8502` | Meta payment method (UPI/Card) rejected for this WABA. | Inform user of Meta billing issue. Use fallback link. |
| `ERR_COMM_CATALOG_SYNC`    | `8503` | Failed to push catalog updates to Meta Business Manager. | Retry sync via queue. Alert admin on persistent failure. |

### Background Services & Scheduled Tasks (SLA)
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_SLA_ASSIGN_TIMEOUT`  | `1801` | Contact remained in unassigned queue beyond 5-minute SLA. | Escalation trigger. Send NATS event to Manager/Bot. |
| `ERR_SLA_CAMPAIGN_STALL`  | `1802` | Bulk worker stopped processing for >10 minutes. | Critical infra alert. Restart worker node. |
| `ERR_SLA_CLEANUP_FAIL`    | `1803` | Periodic DB vacuum/cleanup job failed to run. | Check DB disk space. Log incident to Ops. |

### Configuration & Secret Management
| Internal Code | Code | Description | Strategy |
|---|---|---|---|
| `ERR_CONF_SECRET_WEAK`    | `9101` | Provided JWT secret is <32 chars in production mode. | **FATAL**. Block server startup. Log security threat. |
| `ERR_CONF_ENV_MISMATCH`   | `9102` | Production flags enabled in development environment. | Warn on startup. Prevent critical write actions. |
| `ERR_CONF_MIGRATE_FAIL`   | `9103` | Database schema migration failed during startup. | **FATAL**. Rollback DB. Do not start server. |
| `ERR_CONF_FILE_NOT_FOUND` | `9104` | Specified configuration file path does not exist. | **FATAL**. Check --config flag or environment path. |
| `ERR_CONF_PARSE_FAIL`     | `9105` | TOML / Environment variable parsing failure. | **FATAL**. Check syntax in toml or env naming. |
| `ERR_CONF_KEY_MISSING`    | `9106` | Required API Key (OpenAI/Meta) or Secret missing in Prod. | **FATAL**. Validation failed for critical key. |
| `ERR_CONF_BINARY_MISSING` | `9107` | External binary (Piper/Opusenc) not found at path. | **FATAL**. Check config paths for external tools. |
| `ERR_CONF_TLS_REQUIRED`   | `9108` | Redis/Postgres connection requires TLS but it is disabled. | **SECURITY**. Enforce TLS in production environments. |
| `ERR_CONF_ENCRYPT_KEY_WEAK`| `9109` | AES-256 encryption key for secrets is < 32 chars in Prod. | **FATAL**. Block startup. Secrets must be cryptographically secure. |
| `ERR_CONF_CORS_MISCONFIG` | `9110` | Allowed Origins is empty or wildcard (*) in production. | **SECURITY**. Restrict CORS origins to trusted domains only. |
| `ERR_CONF_ICE_MISSING`    | `9111` | Calling enabled but NO ICE/STUN/TURN servers configured. | Calls will fail for most users. Check calling.ice_servers configuration. |
| `ERR_CONF_PORT_INVALID`   | `9112` | Server Port is out of range (1-65535) or reserved (<1024). | **FATAL**. Correct the server.port setting. |
| `ERR_CONF_DB_INCOMPLETE`  | `9113` | Database host, user, or name is missing in config. | **FATAL**. Ensure all database connection strings are set. |
| `ERR_CONF_JWT_UNSET`      | `9114` | JWT secret is missing or empty in config. | **FATAL**. Authentication will fail globally. |
| `ERR_CONF_WA_TOKEN_UNSET` | `9115` | WhatsApp Webhook Verify Token is missing. | Webhook verification will always fail. |
| `ERR_CONF_STORAGE_PATH`   | `9116` | Storage path is inaccessible or does not have write permissions. | **FATAL**. Ensure the app can write to the storage directory. |
| `ERR_CONF_STORAGE_S3_ERR` | `9117` | S3 credentials or bucket name provided but are invalid/incomplete. | Attachments and recordings will fail to upload. |
| `ERR_CONF_ADMIN_DEFAULT`  | `9118` | Default admin credentials used in a non-development environment. | **SECURITY**. Change admin password immediately. |
| `ERR_CONF_COOKIE_INSECURE`| `9119` | Cookie Secure flag is false in production. | **SECURITY**. Enforced HTTPS required for session cookies. |
| `ERR_CONF_RL_DISABLED`    | `9120` | Rate limiting is disabled in production. | **WARNING**. System is vulnerable to brute-force attacks. |
| `ERR_CONF_VOICE_ASSET`    | `9121` | Required voice asset (hold music/ringback) not found on disk. | Calling flows will have silent audio failures. |
| `ERR_CONF_VOICE_PORT`     | `9122` | WebRTC UDP port range is too narrow or overlaps system reserved. | Media stream negotiation will fail for concurrent calls. |
| `ERR_CONF_VOICE_IP_MISSING`| `9123` | Public IP is missing in cloud/NAT environment. | WebRTC sessions will fail to establish media relay. |
| `ERR_CONF_AI_KEY_UNSET`   | `9124` | AI feature enabled but relevant provider key (OpenAI/Anthropic/Google) is missing. | AI response generation will return 500. |
| `ERR_CONF_TTS_MODEL`      | `9125` | Piper ONNX voice model file not found at configured path. | IVR greetings will fail to generate. |

---

## Principle Guidelines for Developers
1. **Never Return Stack Traces to the Client**: Stack traces belong in Sentry/ClickHouse. Only return the `ERR_` code and a human-readable `message`.
2. **Idempotency is King**: Whenever you throw an error in a webhook or payment route, ensure that a retry won't cause double-processing.
3. **Actionable Errors**: If the error requires the user to do something (e.g., `ERR_WA_POLICY_BAN`), set `actionable: true` in the JSON response payload to trigger the specific UI prompt.
