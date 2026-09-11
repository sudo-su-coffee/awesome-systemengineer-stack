# WhatsApp Business Platform (Cloud API) — A–Z Feature Reference for Universal SaaS
## Complete Source of Truth — v3.0 (Updated April 2026)

> **Scope**: WhatsApp Business Platform (Cloud API) + WhatsApp Manager behavior (Meta).
> This document is the single authoritative reference for every WhatsApp platform integration concern in the SaaS platform.
> **v3.0 additions**: Marketing Messages (MM) API, Conversational Components (Welcome Messages / Ice Breakers / Commands), Interactive Media Carousel messages, Address messages, Location Request messages, Template TTL customization, Template Library, Template Pacing detail, Coupon Code & Limited-Time Offer templates, Address Messages, new Webhook fields (`user_preferences`, `template_category_update`, `message_template_quality_update`, `account_review_update`, `payment_configuration_update`), Groups API full detail, Embedded Signup v4, Multi-Partner Solutions, SIP Calling, Business Encryption API, Conversions API (CAPI) full payload, Brazil Payments (PIX, Boleto, One-Click), Pre-Verified Phone Numbers, WABA Activities API, Schedules API, updated docs index, and expanded 2026 action items table.

---

## Official Docs Index (Start Here)

| Area | URL |
|---|---|
| Hub | `https://developers.facebook.com/documentation/business-messaging/whatsapp/` |
| Cloud API Get Started | `https://developers.facebook.com/docs/whatsapp/cloud-api/get-started` |
| Access Tokens | `https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens/` |
| Messages API (core send) | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api/` |
| Webhooks Setup | `https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/create-webhook-endpoint/` |
| Webhook Payload Schema | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/webhooks/whatsapp-incoming-webhook-payload` |
| Status Webhook Reference | `https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status/` |
| Templates Overview | `https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview` |
| Template TTL | `https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/time-to-live/` |
| Template Pacing | `https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-pacing/` |
| Template Library | `https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-library/` |
| Coupon Code Templates | `https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/coupon-templates/` |
| Limited-Time Offer Templates | `https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/limited-time-offer-templates/` |
| Media Card Carousel Templates | `https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/media-card-carousel-templates/` |
| Messaging Limits | `https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits` |
| Pricing | `https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing` |
| Error Codes | `https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes` |
| Media Upload | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/media/` |
| Flows Getting Started | `https://developers.facebook.com/docs/whatsapp/flows/gettingstarted/` |
| Interactive Flow Messages | `https://developers.facebook.com/docs/whatsapp/cloud-api/messages/interactive-flow-messages/` |
| Interactive Media Carousel | `https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/interactive-media-carousel-messages/` |
| Address Messages | `https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/address-messages/` |
| Location Request Messages | `https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/location-request-messages/` |
| Calling | `https://developers.facebook.com/documentation/business-messaging/whatsapp/calling` |
| Calling SIP | `https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip/` |
| Call Button Messages | `https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links/` |
| Marketing Messages (MM) API | `https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview/` |
| MM API Send | `https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages/` |
| MM API Features | `https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/features/` |
| Conversational Components | `https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/conversational-components/` |
| Conversational Automation API | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/conversational-automation-api` |
| Catalogs Overview | `https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview/` |
| Product Carousel (Catalog) | `https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/interactive-product-carousel-messages/` |
| Groups | `https://developers.facebook.com/documentation/business-messaging/whatsapp/groups` |
| Groups API Reference | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/groups/` |
| QR Codes | `https://developers.facebook.com/documentation/business-messaging/whatsapp/qr-codes/` |
| Business Profile | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-profile-api` |
| Send Messages Guide | `https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages` |
| Mark as Read | `https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/mark-message-as-read/` |
| Typing Indicators | `https://developers.facebook.com/documentation/business-messaging/whatsapp/typing-indicators/` |
| Phone Registration | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/phone-number-registration` |
| Embedded Signup Overview | `https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview/` |
| Embedded Signup v4 | `https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4/` |
| Multi-Partner Solutions | `https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions/` |
| Block Users | `https://developers.facebook.com/docs/whatsapp/cloud-api/block-users/` |
| Business Encryption API | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/business-encryption-api` |
| WABA Activities API | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-activities-api` |
| Schedules API | `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/schedules-api` |
| Pre-Verified Numbers | `https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/pre-verified-numbers/` |
| Payments India | `https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/overview/` |
| Payments Brazil | `https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/overview/` |
| Conversions API | `https://developers.facebook.com/docs/marketing-api/conversions-api/` |
| Platform Status | `https://metastatus.com/whatsapp-business-api` |
| Graph API Changelog | `https://developers.facebook.com/docs/graph-api/changelog` |
| WhatsApp Changelog | `https://developers.facebook.com/docs/whatsapp/cloud-api/support/changelog` |

---

## A) Accounts, IDs, and "What Lives Where"

### Core IDs & Terms

| Term | Description |
|---|---|
| `WABA_ID` | WhatsApp Business Account ID. Appears in webhooks as `entry.id`. Used for tenant mapping. |
| `PHONE_NUMBER_ID` | Business phone number node ID. Used in all send calls: `/{PHONE_NUMBER_ID}/messages`. Also in webhook `metadata.phone_number_id`. |
| `WA_MESSAGE_ID` | WhatsApp message ID (`wamid.HBgL...`). Used for de-dupe and status tracking. |
| `BUSINESS_ID` | Meta Business Manager / Business Portfolio ID. Used for analytics, catalog, WABA management calls. |
| `BSUID` | Business-Scoped User ID. Replaces phone number as recipient identifier for users opting into WhatsApp Usernames (2026 rollout). Delivered via webhooks in `contacts[].wa_id`. Route to `recipient` payload key, NOT `to`. |
| `API_VERSION` | Graph API version (e.g. `v21.0`, `v22.0`). **Pin this in config and only bump as a planned release.** Current stable: `v21.0`. |

### Base URL
```
https://graph.facebook.com/{Version}/...
```

### 2026 Platform Identity Change: WhatsApp Usernames + BSUID
Meta is rolling out WhatsApp Usernames (Q1–Q2 2026). Key impacts for The Platform:
- Users can opt to hide their phone number by default.
- Businesses receive a `BSUID` (Business-Scoped User ID) in webhook `contacts[].wa_id` instead of a phone number.
- `BSUID` is used as the `recipient` key (not `to`) in outbound payloads.
- Businesses that already have a user's phone number from prior interactions can still message them by phone number.
- **SaaS Action**: Extend your contact model to support both phone-number-keyed and BSUID-keyed contacts. Never assume `wa_id` is a phone number.

---

## B) Architecture Mapping (SaaS Platform → WhatsApp Platform)

| SaaS Feature | Platform Mechanism |
|---|---|
| Inbox / Chat | Webhooks (incoming) + `/{PHONE_NUMBER_ID}/messages` (outgoing) |
| Conversation List | Built from message + status events you store. WhatsApp has no "list chats" API. |
| Message Statuses | Webhook `messages` → `statuses[]` (sent/delivered/read/failed/played) |
| Templates | WhatsApp Manager + Templates API. Required outside customer service window. |
| Media Inbound | Receive `media.id` → download → store in ObjectStore → serve via CDN |
| Media Outbound | Upload → get `media_id` → send in message payload |
| Flows (SDUI) | **The Standard for In-App Logic.** Send `type: "interactive"` with `interactive.type: "flow"`. Local native rendering driven by server-sent Flow JSON. |
| Calling | Call permission request templates + `/calls` endpoint + calling webhooks |
| Catalogs | Link WABA to Meta catalog → send product/catalog messages → receive commerce webhooks |
| Groups | Programmatic create + invite link + group messaging (OBA eligibility required) |
| Analytics | `/{BusinessID}?fields=analytics.start().end().granularity()` |
| Marketing Campaigns | `/{PHONE_NUMBER_ID}/marketing_messages` endpoint (MM API) for optimized delivery |
| Welcome Messages | Conversational Components API → `request_welcome` webhook → respond with greeting |
| Ice Breakers | Conversational Automation API → prompts array → received as normal text messages |
| Commands | Conversational Automation API → commands array → user types `/command` |

---

## C) Credentials, Tokens, and Permissions

### Token Types

| Type | Lifetime | Use Case |
|---|---|---|
| System User Access Token | Long-lived / never expires (if set to never) | Best when you control the WABA(s) directly |
| Business Integration System User Access Token | Long-lived | For Embedded Signup / Tech Provider onboarding of customer WABAs |
| Temporary App Token | ~1 hour | Dev/test only. Never use in production. |

### Required Permissions (Typical)
- `business_management`
- `whatsapp_business_management`
- `whatsapp_business_messaging`

### SaaS Rules
- Tokens are **server-only secrets**. Never ship to frontend.
- Store tokens encrypted at rest (AES-256 minimum).
- Use the [Access Token Debugger](https://developers.facebook.com/tools/debug/accesstoken/) to verify scope on any suspect token.
- Alert on `error.code` `190` (token expired) and `10`/`3` (permission removed).
- Implement token rotation as a managed ops workflow.

---

## D) Delivery Model + Reliability (Non-Negotiable)

### Webhook Contract

| Property | Behavior |
|---|---|
| Delivery guarantee | At-least-once. Webhooks can be **duplicated**, **batched**, and **re-ordered**. |
| Retry window | Meta retries for up to **7 days** on non-200 responses (see Section X for edge cases). |
| Response timeout | **5 seconds** hard limit. Always respond 200 immediately and process async. |
| Batch size | Multiple events may arrive in a single POST payload. Process all `entry[].changes[]`. |
| Ordering | NOT guaranteed. Status updates for a single message can arrive out of order. |
| Max payload size | **3 MB** per webhook POST. |

### Webhook Priority (Phone Number vs. WABA Level)
- **Primary**: Phone Number Webhook (overrides WABA webhook if set).
- **Secondary**: WABA-level Webhook (used only if no phone number webhook configured).
- **Fallback**: Empty response (no delivery).

### Webhook Verification (GET)
```
GET /your-webhook?hub.mode=subscribe&hub.challenge=<challenge>&hub.verify_token=<your_token>
```
Validate `hub.verify_token`. Respond `200` with `hub.challenge` as body.

### Webhook Signature Validation (POST)
```
X-Hub-Signature-256: sha256=<HMAC-SHA256(raw_body, APP_SECRET)>
```
- Validate over **raw request body** using your **App Secret**.
- Reject any request with an invalid or missing signature.

### Idempotency Rules (Minimum)
- **Inbound de-dupe key**: `(PHONE_NUMBER_ID, WA_MESSAGE_ID)` for messages; `(PHONE_NUMBER_ID, status_id, status)` for status updates.
- **Outbound idempotency**: Require `X-Idempotency-Key` header at your API boundary and enforce server-side so UI retries never double-send.
- Store de-dupe keys in Redis with a TTL of 48 hours minimum.

### Minimum Persistence Per Webhook
- Receive timestamp + your `request_id`
- `WABA_ID` + `PHONE_NUMBER_ID` → tenant mapping
- De-dupe key used + decision (processed / ignored)
- `fbtrace_id` if present on any error

---

## E) Core Endpoints Reference

### Base: `POST https://graph.facebook.com/{Version}/{PHONE_NUMBER_ID}/messages`
Headers:
```http
Authorization: Bearer <ACCESS_TOKEN>
Content-Type: application/json
```

| # | Endpoint | Method | Purpose |
|---|---|---|---|
| 1 | `/{VERSION}/{PHONE_NUMBER_ID}/messages` | POST | Send any message |
| 2 | `/{VERSION}/{PHONE_NUMBER_ID}/whatsapp_business_profile` | GET / POST | Business profile read/update |
| 3 | `/{VERSION}/{PHONE_NUMBER_ID}/register` | POST | Register phone number for messaging |
| 4 | `/{VERSION}/{PHONE_NUMBER_ID}/deregister` | POST | Deregister phone number |
| 5 | `/{VERSION}/{PHONE_NUMBER_ID}/request_code` | POST | Request OTP for verification |
| 6 | `/{VERSION}/{PHONE_NUMBER_ID}/verify_code` | POST | Verify OTP for registration |
| 7 | `/{VERSION}/{PHONE_NUMBER_ID}/qr_codes` | GET / POST | List or create QR codes |
| 8 | `/{VERSION}/{PHONE_NUMBER_ID}/qr_codes/{QR_CODE_ID}` | DELETE | Delete a QR code |
| 9 | `/{VERSION}/{BUSINESS_ID}/flows` | POST | Create a Flow |
| 10 | `/{VERSION}/{FLOW_ID}/assets` | POST | Upload Flow JSON asset |
| 11 | `/{VERSION}/{FLOW_ID}/publish` | POST | Publish a Flow |
| 12 | `/{VERSION}/{FLOW_ID}/deprecate` | POST | Deprecate a Flow |
| 13 | `/{VERSION}/{FLOW_ID}` | DELETE | Delete a Flow |
| 14 | `/{VERSION}/{BUSINESS_ID}/owned_product_catalogs` | GET / POST | Manage catalogs |
| 15 | `/{VERSION}/{CATALOG_ID}/products` | POST | Add product to catalog |
| 16 | `/{VERSION}/{BUSINESS_ID}?fields=analytics...` | GET | Messaging analytics |
| 17 | `/{VERSION}/{BUSINESS_ID}/template_analytics` | GET | Template analytics |
| 18 | `/{VERSION}/{MEDIA_ID}` | GET | Get media download URL |
| 19 | `/{VERSION}/{PHONE_NUMBER_ID}/media` | POST | Upload media |
| 20 | `/{VERSION}/{MEDIA_ID}` | DELETE | Delete uploaded media |
| 21 | `/{VERSION}/{PHONE_NUMBER_ID}/calls` | POST | Call actions (accept/reject/terminate) |
| 22 | `app/uploads` | POST | Start resumable upload session (profile pics, template media headers) |
| 23 | `/{VERSION}/{PHONE_NUMBER_ID}/marketing_messages` | POST | Send marketing message via MM API (optimized delivery) |
| 24 | `/{VERSION}/{WABA_ID}/conversational_automation` | GET / POST | Read/set Conversational Components (welcome messages, ice breakers, commands) |
| 25 | `/{VERSION}/{PHONE_NUMBER_ID}/block_users` | POST / DELETE / GET | Block / unblock / list blocked users |
| 26 | `/{VERSION}/{WABA_ID}/message_templates` | GET / POST | List and create templates |
| 27 | `/{VERSION}/{TEMPLATE_ID}` | POST / DELETE | Update or delete a specific template |
| 28 | `/{VERSION}/{WABA_ID}/activities` | GET | WABA activity log |
| 29 | `/{VERSION}/{WABA_ID}/scheduled_messages` | GET / POST | List or schedule messages (Schedules API) |
| 30 | `/{VERSION}/{PHONE_NUMBER_ID}/business_encryption` | GET / POST | Read/set business encryption public key |
| 31 | `/{VERSION}/{WABA_ID}/phone_numbers` | GET | List all phone numbers in WABA |
| 32 | `/{VERSION}/{PHONE_NUMBER_ID}` | GET | Get phone number details |
| 33 | `/{VERSION}/{WABA_ID}/subscribed_apps` | POST / GET / DELETE | Subscribe / list / unsubscribe webhooks |
| 34 | `/{VERSION}/{WABA_ID}` | GET | Get WABA details |
| 35 | `/{VERSION}/{WABA_ID}/groups` | POST | Create a WhatsApp group |
| 36 | `/{VERSION}/{GROUP_ID}` | GET / POST | Get group info / update group |
| 37 | `/{VERSION}/{GROUP_ID}/participants` | POST | Add/remove group participants |
| 38 | `/{VERSION}/{GROUP_ID}/invite_link` | GET | Get group invite link |
| 39 | `/{VERSION}/{GROUP_ID}/invite_link` | DELETE | Reset group invite link |
| 40 | `/{PIXEL_ID}/events` | POST | Send Conversions API (CAPI) event |

---

## F) Media: Upload, Download, Limits, and Storage Rules

### Supported Media Types & Size Limits

| Type | Formats | Max Upload Size | Post-Processing Limit |
|---|---|---|---|
| Image | `image/jpeg`, `image/png`, `image/webp` | 64 MB | 5 MB |
| Video | `video/mp4`, `video/3gp` | 64 MB | 16 MB |
| Audio | `audio/aac`, `audio/mp4`, `audio/mpeg`, `audio/amr`, `audio/ogg` | 64 MB | 16 MB |
| Document | `application/pdf`, `application/vnd.ms-powerpoint`, `application/msword`, `application/vnd.ms-excel`, `application/vnd.openxmlformats-officedocument.*`, `text/plain` | 64 MB | 100 MB |
| Sticker | `image/webp` | 64 MB | 100 KB (must be 512×512px, transparent bg) |
| GIF | `image/gif` | 64 MB | 5 MB (supported in MM API, treated as image) |

**Note**: WhatsApp will reject messages if post-processing size exceeds the limits above, even if the upload succeeded.

### Media Upload (Outbound)
```http
POST https://graph.facebook.com/{Version}/{PHONE_NUMBER_ID}/media
Content-Type: multipart/form-data
Authorization: Bearer <ACCESS_TOKEN>

messaging_product=whatsapp
file=@/path/to/file
type=image/jpeg
```
Returns:
```json
{ "id": "<MEDIA_ID>" }
```
Use `MEDIA_ID` in your send payload. Media uploaded this way expires and must be re-uploaded after **30 days**.

### Media Download (Inbound)
1. Receive `messages[].image.id` (or `video.id`, `audio.id`, etc.) from webhook.
2. GET `/{VERSION}/{MEDIA_ID}` → returns a `url` field.
3. Download that URL immediately. **Media URLs expire in ~5 minutes.**
4. Store in your ObjectStore (S3/GCS/R2). Serve via presigned URL or CDN.
5. If the URL has expired, re-fetch: GET `/{VERSION}/{MEDIA_ID}` again.
6. Meta stores inbound media for **14 days** on their servers.

### Resumable Upload API (Profile Pics & Template Headers Only)
Used for: setting Business Profile pictures and creating template media headers (not for sending regular media messages).
```
Step 1: POST /app/uploads → get upload session ID
Step 2: POST /{UPLOAD_SESSION_ID} with file bytes → get "h" file handle
Step 3: Use "h" handle in profile or template API call
```
**Important**: Resumable upload returns a file `handle` (h value), NOT a WhatsApp `media_id`. Do not mix these up.

### SaaS Storage Rules
- **Never** proxy raw Meta media URLs to the client.
- **Never** store raw Meta media URLs in your DB. They expire.
- Always download + re-upload to your ObjectStore, then serve via your own CDN/presigned URLs.
- Treat all media downloads as retryable with exponential backoff + re-fetch-on-expiry.
- Media delete endpoint: `DELETE /{VERSION}/{MEDIA_ID}` (cleanup after storing).

---

## G) Messaging Limits, Tiers, and Quality Rating

### Messaging Limit Overview (2026)
Messaging limits control how many **unique** recipients your business can initiate messages to (outside the customer service window) in a rolling 24-hour period.

**As of Q2 2026, Meta has simplified tiers:**

| Path | Limit |
|---|---|
| New business portfolio (unverified) | 250 unique recipients/day |
| After Business Verification OR quality scaling | 100K unique recipients/day (direct — intermediate 2K/10K tiers removed) |
| Unlimited tier | Requires higher scale + Meta approval |

**Key changes**: The 2K and 10K intermediate tiers are removed (full removal Q2 2026). Verified businesses jump straight to 100K/day.

### Limits Are Portfolio-Level (Not Per Number)
- Messaging limits are set at the **business portfolio** level and shared by **all phone numbers** within that portfolio.
- One number can consume the entire portfolio's daily limit.
- Plan multi-number deployments accordingly.

### Quality Rating
Your phone number's quality rating is based on recent user feedback (blocks, reports). It directly controls your effective messaging tier:

| Rating | Color | Behavior |
|---|---|---|
| High | Green | Full tier in effect |
| Medium | Yellow | Tier may be restricted |
| Low | Red | Tier downgraded; may be flagged/limited |

**Quality Scoring Signals**:
- Users blocking your number
- Users reporting your messages as spam
- Users opting out / replying STOP

**Tier Upgrade Logic**: Meta auto-upgrades tiers when your number consistently sends near your current limit with a High quality rating.

**Monitoring**: Subscribe to `phone_number_quality_update` webhook. Also check quality in WhatsApp Manager.

### Portfolio Pacing (2026 — New)
Meta is rolling out **portfolio pacing**: large campaigns are delivered in batches. Between batches, Meta monitors feedback signals. If signals are negative, remaining batches are paused.
- **Impact on The Platform**: Campaigns may not deliver all at once even under 100K/day limit.
- **SaaS Action**: Build campaign status tracking, not just "sent vs. pending." Show "delivery paused" states.
- Meta's internal guardrail: even paced campaigns should deliver within 1 hour at 99th percentile.

### Per-User Marketing Template Limits
Independent of tier, WhatsApp enforces per-user marketing limits: a single user can only receive a capped number of marketing template messages from **all businesses combined** in a rolling period.
- Hits return `error.code: 131048`.
- As of 2026: an estimated daily cap of 2 marketing messages per user from any single business, with a combined cross-business limit.
- **US users**: As of April 1, 2025, marketing template delivery to US phone numbers is temporarily paused. US-registered business phone numbers can still send marketing to non-US users.

---

## H) Pricing Model (Updated July 2025 — Per-Message)

### Model Change: CBP → PMP
**Effective July 1, 2025**, Meta moved from Conversation-Based Pricing (CBP) to **Per-Message Pricing (PMP)**. You are now charged **per delivered template message**, not per 24-hour conversation window.

### Message Categories & Charging Rules

| Category | When Charged | Free Conditions |
|---|---|---|
| **Marketing** | Per message, always charged | Only free during 72-hour FEP window |
| **Utility** | Per message | Free during active 24-hour customer service window (CSW) or 72-hour FEP window |
| **Authentication** | Per message | Free during 72-hour FEP window; `authentication-international` is a premium sub-rate |
| **Service** | Never charged | Always free (unlimited, no monthly cap as of Nov 1, 2024) |

### Customer Service Window (CSW)
- Opens when a user **messages, calls, or accepts a call** from your business.
- Lasts **24 hours**, reset on each new user message.
- During an active CSW: utility templates and free-form messages are **free**.
- Marketing and authentication templates are **always charged** regardless of CSW.

### Free Entry Point (FEP) Window — 72 Hours
Triggered when a user messages you via:
- Click-to-WhatsApp ad (Facebook/Instagram)
- Facebook Page call-to-action button

During the 72-hour FEP window, **all template types (including marketing) are free**. This is the most cost-effective way to run campaigns.

### Volume Tiers (Utility & Authentication Only)
- As monthly chargeable message volume grows, per-message rates automatically decrease.
- Volume tiers reset monthly.
- Only **chargeable** utility/authentication messages count (free messages during CSW/FEP do not count).
- Up to ~20% discount at high volumes.
- Marketing messages have **flat pricing** — no volume discounts.

### Pricing in `statuses` Webhook
Every status update includes a `pricing` node:
```json
"pricing": {
  "billable": true,
  "pricing_model": "PMP",
  "category": "marketing"
}
```
**Post-July 2025 categories in pricing webhook**:
- `marketing`
- `marketing_lite` (MM API sends)
- `utility`
- `authentication`
- `authentication_international`
- `service`
- `referral_conversion` (free — Click-to-WhatsApp ads)

**SaaS Action**: Parse and store the `pricing` node from every status webhook. This is your billing audit trail for tenants.

### Service Conversations: Fully Free (Nov 2024 Update)
As of November 1, 2024, **all service conversations are free with no monthly cap**. The previous 1,000 free/month limit is removed. React within the CSW with any free-form or utility message at zero Meta cost.

---

## I) Error Codes — Complete Reference

### Error Response Schema
```json
{
  "error": {
    "message": "(#131030) Recipient phone number not in allowed list",
    "type": "OAuthException",
    "code": 131030,
    "error_subcode": 123456,
    "error_user_msg": "User-facing message if relevant",
    "error_data": {
      "messaging_product": "whatsapp",
      "details": "Additional detail string"
    },
    "fbtrace_id": "A1b2c3d4e5f6g7h8i9j0"
  }
}
```

**Handling rules**:
1. Route on `error.code` (integer), not HTTP status or `type`.
2. Always log `fbtrace_id` — Meta support requires it for investigations.
3. Check `error_subcode` for additional context on complex errors.
4. `error_user_msg` can surface to end-users safely when present.

---

### Authorization & OAuth Errors

| Code | Name | Cause | The Platform Action |
|---|---|---|---|
| `0` | Auth Exception | App could not authenticate user. Token expired/invalidated, or user revoked access. | Regenerate access token. Alert ops. |
| `3` | API Method | Missing permissions on the token. HTTP 500 from Meta. | Check token scope. Generate new system user token. |
| `10` | Permission Denied | Required permission not granted or revoked from the app. | Re-grant permissions. Rotate token. |
| `190` | Access Token Expired | Token has passed its expiry. | Generate new system user token. Alert on this immediately. |
| `200–299` | API Permission | Permission exists but is blocked at API feature level. HTTP 403. | Debug token scope. Re-grant. |

---

### Rate Limiting & Throughput Errors

| Code | Name | Cause | The Platform Action |
|---|---|---|---|
| `4` | API Too Many Calls | App exceeded hourly rate limit on WhatsApp Business Management API. Default: 200 req/hr for unlinked apps; 5,000 req/hr for linked accounts. | Implement exponential backoff + jitter. Queue requests. |
| `130429` | Cloud API Throughput Reached | Exceeded per-second message throughput. Default: 80 mps. Eligible accounts: 1,000 mps. | Throttle sends. Use a rate limiter. Request higher throughput if needed. |
| `131048` | Spam Rate Limit Hit | Too many users blocked/reported messages from this number. Quality rating dropped. | Reduce send volume. Review content quality. Do not retry — wait for quality recovery. |
| `131056` | Pair Rate Limit Hit | Too many messages from your number to the same recipient in a short period. | Do not retry to that recipient. Wait. Continue sending to other recipients. |
| `133016` | Registration Rate Limit Exceeded | >10 registration/deregistration requests for same phone number within 72-hour window. | Wait 72 hours. Do not retry. |

---

### Integrity & Account Restriction Errors

| Code | Name | Cause | The Platform Action |
|---|---|---|---|
| `368` | Temporarily Blocked — Policy Violation | WABA restricted for violating WhatsApp Business Messaging Policy, Commerce Policy, or ToS. Blocks can be 1–30 days or permanent. | Check Business Support Home. File appeal if eligible. Alert tenant immediately. |
| `130497` | Country Restriction | WABA restricted from messaging users in certain countries due to policy on regulated goods. | Inform tenant of geo-restriction. Do not retry. |
| `131031` | Account Locked | WABA disabled for policy violations OR data verification mismatch (e.g. wrong 2-step PIN). | Diagnose via Business Support Home. Reset 2FA PIN if that's the cause. Appeal if policy violation. |

---

### General / Miscellaneous Errors

| Code | Name | Cause | The Platform Action |
|---|---|---|---|
| `1` | API Unknown | Invalid request format OR temporary Meta server error. | Check platform status. Validate request format. Retry with backoff. |
| `2` | API Service | Temporary service downtime or overload on Meta's end. | Check platform status. Wait and retry. |
| `33` | Parameter Invalid (Phone Deleted) | The business phone number in the request has been deleted or is no longer valid. | Verify phone numbers in WABA. Update config. |
| `100` | Invalid Parameter | Request includes unsupported, misspelled, or oversized parameters. Also: public key format mismatch, template name collision. | Review endpoint reference. Fix parameters. |
| `130472` | User in Experiment | User is in Meta's Marketing Message Experiment (~1% of users). Marketing template not delivered. | Do not retry. This is expected behavior. |
| `131000` | Unknown Internal Error | Message failed due to an unknown error on Meta's side. | Retry with exponential backoff. If persistent, contact Meta support with `fbtrace_id`. |
| `131005` | Access Denied | Permission not granted or removed at the Cloud API layer (distinct from Graph-level `10`/`200-299`). | Verify token permissions. Generate new token. |
| `131008` | Required Parameter Missing | A required parameter is absent from the request. | Check endpoint reference. Add missing field. |
| `131009` | Parameter Value Invalid | One or more parameter values are invalid (bad phone format, invalid media ID, bad template name, etc.). | Validate inputs before sending. Check formats. |
| `131016` | Service Unavailable | A downstream service is temporarily unavailable. | Check platform status. Retry. |
| `131021` | Recipient Cannot Be Sender | The `to` number matches your own phone number. | Routing bug in your code. Fix recipient resolution. |
| `131026` | Message Undeliverable | Recipient phone is not a valid WhatsApp number, or temporarily unreachable. | Mark contact as potentially invalid. Do not spam-retry. |
| `131037` | Display Name Approval Needed | WhatsApp-provided number needs display name approval before messages can be sent. | Complete display name approval in WhatsApp Manager. |
| `131042` | Business Eligibility / Payment Issue | Payment account issue on Meta's side. | Check Meta payment method. Resolve billing issue. |
| `131045` | Incorrect Certificate | Certificate used for registration is incorrect. | Re-register the phone number with the correct certificate. |
| `131047` | Re-engagement Message | More than 24 hours have passed since last user reply (outside customer service window). Cannot send free-form message. | Block send in UI. Prompt user to send a Template message instead. |
| `131049` | Meta Chose Not to Deliver | Meta's systems decided not to deliver the message (quality/spam signals, user preferences, per-user marketing limit). | Do not retry. Review content and sending patterns. |
| `131051` | Unsupported Message Type | User sent a message type not supported by Cloud API. | Log and surface as "Unsupported" in UI. No retry needed. |
| `131052` | Media Download Error | Media could not be downloaded (URL expired, file unavailable). | Re-fetch media URL. Implement retry with re-fetch logic. |
| `131053` | Media Upload Error | Media could not be uploaded to Meta. | Retry upload. Check file format and size against limits. |
| `131057` | Account in Maintenance | WABA is temporarily in maintenance mode. | Wait. Retry after a few minutes. Alert tenant. |
| `131060` | Unsupported message type (Coexistence) | Transient error when number is in Coexistence mode. | Wait 5 seconds and retry once. |

---

### Template-Specific Errors

| Code | Name | Cause | The Platform Action |
|---|---|---|---|
| `132000` | Template Param Count Mismatch | Number of variables in the send payload doesn't match the approved template. | Fix variable count. Sync template definitions. |
| `132001` | Template Does Not Exist | Template name/language combo not found or not approved. | Verify template status in WhatsApp Manager. Sync approved templates. |
| `132005` | Template Hydrated Text Too Long | Substituted variable values make the final message exceed character limits. | Truncate variable values before sending. |
| `132007` | Template Format Policy Violated | Template content violates WhatsApp formatting policies. | Review and re-submit template for approval. |
| `132012` | Template Parameter Format Mismatch | Variable type mismatch (e.g. sending text where currency is expected). | Fix parameter types to match template definition. |
| `132015` | Template Paused | Meta's systems paused the template (poor quality signals). | Do not send. Contact Meta or wait for auto-unpause. Monitor `message_template_status_update` webhook. |
| `132016` | Template Disabled | Template has been permanently disabled by Meta. | Stop sending. Create a new compliant template. Alert tenant. |
| `132068` | Flow Blocked | The Flow associated with this message is blocked. | Check Flow status. Do not send until unblocked. |
| `132069` | Flow Throttled | The Flow is temporarily throttled due to high usage. | Retry with backoff. |

---

### Registration & 2-Step PIN Errors

| Code | Name | Cause | The Platform Action |
|---|---|---|---|
| `133000` | Incomplete Deregistration | Previous deregistration was not completed before re-registration was attempted. | Complete the deregistration flow first. |
| `133004` | Server Temporarily Unavailable | Registration service temporarily down. | Retry after a few minutes. |
| `133005` | 2-Step PIN Mismatch | PIN provided during registration doesn't match. | Prompt user for correct PIN. After too many tries, a lockout occurs. |
| `133006` | Phone Number Re-verification Needed | Number requires re-verification before use. | Re-run the verification flow (`request_code` → `verify_code`). |
| `133008` | Too Many PIN Guesses | Too many failed 2-step PIN attempts. | Wait for lockout to expire (usually several hours). |
| `133009` | PIN Guessed Too Fast | Rate limiting on PIN verification attempts. | Add delay between PIN attempts. |
| `133010` | Phone Number Not Registered | Attempting to send from a phone number that isn't registered for Cloud API. | Register the number via `/{PHONE_NUMBER_ID}/register`. |
| `133015` | Please Wait Before Re-registering | Too soon to re-register this number. | Wait before retrying registration. |
| `133016` | Registration Rate Limit | Too many registration/deregistration attempts (>10 in 72 hours). | Wait 72 hours. |

---

### Flow-Specific Errors

| Code | Name | Cause | The Platform Action |
|---|---|---|---|
| `132068` | Flow Blocked | Flow was blocked by Meta. | Check Flow status in WhatsApp Manager. |
| `132069` | Flow Throttled | Flow temporarily rate-limited. | Retry with backoff. |

### Payments / Commerce Errors

| Code | Name | Cause | The Platform Action |
|---|---|---|---|
| `134011` | Payments Terms Not Accepted | Business has not accepted WhatsApp Payments terms. | Tenant must accept terms in WhatsApp Manager. |
| `135000` | Generic User Error | Generic user-level error. | Check `error_data.details` for specifics. Log and retry if applicable. |

---

## J) Webhook Architectures & Payload Reference

### Envelope (All Webhooks)
```json
{
  "object": "whatsapp_business_account",
  "entry": [
    {
      "id": "<WABA_ID>",
      "changes": [
        {
          "value": {
            "messaging_product": "whatsapp",
            "metadata": {
              "display_phone_number": "15551234567",
              "phone_number_id": "<PHONE_NUMBER_ID>"
            },
            "contacts": [...],
            "messages": [...],
            "statuses": [...],
            "errors": [...]
          },
          "field": "messages"
        }
      ]
    }
  ]
}
```

The `field` property tells you what type of change this is:
- `"messages"` — inbound messages and outbound status updates
- `"message_template_status_update"` — template approval/rejection/pause/disable
- `"message_template_quality_update"` — template quality score changed
- `"template_category_update"` — template category re-classified by Meta
- `"phone_number_quality_update"` — quality rating changes
- `"phone_number_name_update"` — display name approval status
- `"account_alerts"` — WABA account health alerts
- `"account_review_update"` — WABA review status changed
- `"account_update"` — WABA state changes
- `"business_capability_update"` — messaging tier / max phones changed
- `"security"` — security events
- `"flows"` — Flow endpoint health
- `"calls"` — call lifecycle events
- `"user_preferences"` — user messaging preference changes
- `"payment_configuration_update"` — payment gateway config changes

---

### Inbound Message Payloads

#### 1) Text Message
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876543",
  "type": "text",
  "text": { "body": "Hello there!" }
}
```

#### 2) Image / Video / Audio / Document
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "image",
  "image": {
    "mime_type": "image/jpeg",
    "sha256": "<hash>",
    "id": "<MEDIA_ID>"
  }
}
```
Replace `"image"` with `"video"`, `"audio"`, or `"document"` for other types. Documents include a `"filename"` field.

#### 3) Sticker
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "sticker",
  "sticker": {
    "mime_type": "image/webp",
    "sha256": "<hash>",
    "id": "<MEDIA_ID>",
    "animated": false
  }
}
```

#### 4) Interactive — Button Reply
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876546",
  "type": "interactive",
  "interactive": {
    "type": "button_reply",
    "button_reply": {
      "id": "unique-button-payload",
      "title": "Yes"
    }
  }
}
```

#### 5) Interactive — List Reply
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876546",
  "type": "interactive",
  "interactive": {
    "type": "list_reply",
    "list_reply": {
      "id": "opt_1",
      "title": "Option 1",
      "description": "Desc 1"
    }
  }
}
```

#### 6) Interactive — Flow Response (nfm_reply)
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "interactive",
  "interactive": {
    "type": "nfm_reply",
    "nfm_reply": {
      "response_json": "{\"form_field_1\":\"value_1\",\"form_field_2\":\"value_2\"}",
      "body": "User submitted form",
      "name": "flow_name"
    }
  }
}
```
`response_json` is a **JSON string** — parse it after receiving. Key it with your `flow_token` to retrieve session state.

#### 7) Location
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "location",
  "location": {
    "latitude": "37.484213",
    "longitude": "-122.143199",
    "name": "Optional place name",
    "address": "Optional address"
  }
}
```

#### 8) Contacts
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "contacts",
  "contacts": [
    {
      "name": { "first_name": "John", "formatted_name": "John Doe" },
      "phones": [{ "phone": "15551234567", "wa_id": "15551234567", "type": "HOME" }]
    }
  ]
}
```

#### 9) Reaction
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "reaction",
  "reaction": {
    "message_id": "wamid.HBgL...",
    "emoji": "👍"
  }
}
```
If `emoji` is empty string, the user removed their reaction.

#### 10) Order (Commerce)
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "order",
  "order": {
    "catalog_id": "<CATALOG_ID>",
    "product_items": [
      {
        "product_retailer_id": "<SKU_1>",
        "quantity": "2",
        "item_price": "10.00",
        "currency": "USD"
      }
    ],
    "text": "Please arrange delivery."
  }
}
```

#### 11) System (Number Changed, Identity Update)
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "system",
  "system": {
    "body": "User changed their phone number.",
    "identity": "<hash_value>",
    "type": "customer_changed_number",
    "customer": "16661234567"
  }
}
```

#### 12) Unsupported
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "unsupported",
  "errors": [
    {
      "code": 131051,
      "title": "Unsupported message type",
      "message": "Message type is not supported."
    }
  ]
}
```

#### 13) Click-to-WhatsApp Referral (Ad Attribution)
Any inbound message initiated via an ad will include a `referral` node:
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "text",
  "text": { "body": "I saw this ad!" },
  "referral": {
    "source_url": "https://fb.me/...",
    "source_id": "<AD_ID>",
    "source_type": "ad",
    "headline": "Holiday Sale",
    "body": "Buy 1 get 1 free",
    "video_url": "https://video-url.mp4",
    "image_url": "https://image-url.jpg"
  }
}
```
Presence of `referral` means a 72-hour FEP free window is now active.

#### 14) Context / Reply-to (Message Threading)
When a user replies to or forwards a specific message:
```json
"context": {
  "from": "<USER_WA_ID>",
  "id": "<ORIGINAL_MESSAGE_ID>",
  "forwarded": true,
  "frequently_forwarded": false
}
```

#### 15) Welcome Request (Conversational Components)
Fired when welcome messages are enabled and a user opens a chat for the first time:
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "request_welcome"
}
```
This opens a CSW. Respond with your greeting message. This fires only once per user per thread (resets if the user deletes the chat thread).

#### 16) Edit Message (User Edits a Previously Sent Message)
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "text",
  "text": { "body": "Edited text content" },
  "context": {
    "from": "<USER_WA_ID>",
    "id": "<ORIGINAL_MESSAGE_ID>",
    "edited": true
  }
}
```

---

### Status Update Payload (Outbound Message Lifecycle)

Status hierarchy: `sent` → `delivered` → `read`. Failed: `failed`.

```json
{
  "statuses": [
    {
      "id": "wamid.HBgL...",
      "status": "delivered",
      "timestamp": "1669876544",
      "recipient_id": "<USER_WA_ID>",
      "conversation": {
        "id": "<CONVERSATION_ID>",
        "expiration_timestamp": "1669963200",
        "origin": { "type": "user_initiated" }
      },
      "pricing": {
        "billable": true,
        "pricing_model": "PMP",
        "category": "service"
      }
    }
  ]
}
```

**For `status: "failed"`**, an `errors` array is included:
```json
{
  "id": "wamid.HBgL...",
  "status": "failed",
  "timestamp": "1669876544",
  "recipient_id": "<USER_WA_ID>",
  "errors": [
    {
      "code": 131026,
      "title": "Message Undeliverable",
      "message": "...",
      "error_data": { "details": "..." }
    }
  ]
}
```

**Origin types** in `conversation.origin.type`:
- `user_initiated` — user sent first
- `business_initiated` — business sent first (template)
- `referral_conversion` — originated from a Click-to-WhatsApp ad (free)

**Media-specific status**: `played` — for audio/video messages that were played by the recipient.

---

### Account-Level Webhooks

#### Phone Number Quality Update
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "phone_number_quality_update",
      "value": {
        "display_phone_number": "15551234567",
        "event": "FLAGGED",
        "current_limit": "TIER_10K"
      }
    }]
  }]
}
```
**Event values**: `FLAGGED`, `UNFLAGGED`, `CONNECTED`, `RESTRICTED`, `DISABLED_UPDATE`, `DELETED`.

#### Template Status Update
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "message_template_status_update",
      "value": {
        "message_template_id": "1234567890",
        "message_template_name": "seasonal_promo",
        "message_template_language": "en_US",
        "event": "APPROVED",
        "reason": ""
      }
    }]
  }]
}
```
**Event values**: `APPROVED`, `REJECTED`, `PENDING_DELETION`, `PAUSED`, `DISABLED`.
On `REJECTED`, check `reason` for the rejection explanation to fix and resubmit.

#### Template Quality Update (NEW)
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "message_template_quality_update",
      "value": {
        "message_template_id": "1234567890",
        "message_template_name": "seasonal_promo",
        "message_template_language": "en_US",
        "previous_quality_score": "GREEN",
        "new_quality_score": "YELLOW"
      }
    }]
  }]
}
```
Quality score values: `GREEN`, `YELLOW`, `RED`.

#### Template Category Update (NEW)
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "template_category_update",
      "value": {
        "message_template_id": "1234567890",
        "message_template_name": "order_update",
        "message_template_language": "en_US",
        "previous_category": "UTILITY",
        "new_category": "MARKETING"
      }
    }]
  }]
}
```
When Meta re-categorizes a template, this fires. **SaaS Action**: Re-sync template definitions and alert tenant, as category change affects billing.

#### WABA Account Alert
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "account_alerts",
      "value": {
        "entity_type": "PHONE_NUMBER",
        "entity_id": "<PHONE_NUMBER_ID>",
        "alert_severity": "WARNING",
        "alert_status": "ACTIVE",
        "alert_type": "ACCOUNT_BANNED",
        "creation_time": 1669876543
      }
    }]
  }]
}
```

#### Account Review Update (NEW)
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "account_review_update",
      "value": {
        "decision": "APPROVED",
        "violation_type": ""
      }
    }]
  }]
}
```
Fired when WABA review status changes. `decision` values: `APPROVED`, `REJECTED`.

#### Payment Configuration Update (NEW)
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "payment_configuration_update",
      "value": {
        "phone_number": "15551234567",
        "payment_configuration": "<PG_CONFIG_TOKEN>",
        "event": "ACTIVE"
      }
    }]
  }]
}
```
Event values: `ACTIVE`, `INACTIVE`. Monitor payment gateway config changes to avoid payment send failures.

#### User Preferences Webhook (NEW)
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "user_preferences",
      "value": {
        "messaging_product": "whatsapp",
        "metadata": {
          "phone_number_id": "<PHONE_NUMBER_ID>"
        },
        "user_preferences": [{
          "wa_id": "<USER_WA_ID>",
          "marketing_stop": true
        }]
      }
    }]
  }]
}
```
`marketing_stop: true` means the user has opted out of marketing messages from this phone number. **SaaS Action**: Immediately add this user to your marketing suppression list for this tenant. Never send marketing templates to opted-out users.

---

## K) Outbound Message Payloads (Complete Reference)

All sent via: `POST https://graph.facebook.com/{Version}/{PHONE_NUMBER_ID}/messages`

### 1) Text
```json
{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "<USER_WA_ID_OR_PHONE>",
  "type": "text",
  "text": {
    "preview_url": false,
    "body": "Hello world!"
  }
}
```

### 2) Image (by Media ID or URL)
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "image",
  "image": {
    "id": "<MEDIA_ID>",
    "caption": "Optional caption text"
  }
}
```
Use `"link": "https://url.com/image.jpg"` instead of `"id"` for URL-based sending. Same pattern for `video`, `audio`, `document`, `sticker`.

### 3) Template (with Variables and Buttons)
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "template",
  "template": {
    "name": "order_confirmation",
    "language": { "code": "en_US" },
    "components": [
      {
        "type": "header",
        "parameters": [
          { "type": "image", "image": { "id": "<MEDIA_ID>" } }
        ]
      },
      {
        "type": "body",
        "parameters": [
          { "type": "text", "text": "John" },
          { "type": "text", "text": "#ORD-1234" },
          { "type": "currency", "currency": { "fallback_value": "$10.00", "code": "USD", "amount_1000": 10000 } },
          { "type": "date_time", "date_time": { "fallback_value": "January 1, 2025" } }
        ]
      },
      {
        "type": "button",
        "sub_type": "quick_reply",
        "index": "0",
        "parameters": [
          { "type": "payload", "payload": "custom-payload-id" }
        ]
      },
      {
        "type": "button",
        "sub_type": "url",
        "index": "1",
        "parameters": [
          { "type": "text", "text": "order-1234" }
        ]
      }
    ]
  }
}
```

### 4) Interactive — Reply Buttons (Max 3)
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "button",
    "header": { "type": "text", "text": "Optional header" },
    "body": { "text": "Do you want to proceed?" },
    "footer": { "text": "Optional footer" },
    "action": {
      "buttons": [
        { "type": "reply", "reply": { "id": "btn_yes", "title": "Yes" } },
        { "type": "reply", "reply": { "id": "btn_no", "title": "No" } }
      ]
    }
  }
}
```

### 5) Interactive — List (Max 10 rows, max 10 sections)
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "list",
    "header": { "type": "text", "text": "Select an option" },
    "body": { "text": "Please choose from the menu below." },
    "footer": { "text": "Optional footer" },
    "action": {
      "button": "View Menu",
      "sections": [
        {
          "title": "Section 1",
          "rows": [
            { "id": "opt_1", "title": "Option 1", "description": "Desc 1" },
            { "id": "opt_2", "title": "Option 2", "description": "Desc 2" }
          ]
        }
      ]
    }
  }
}
```

### 6) Interactive — CTA URL Button
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "cta_url",
    "body": { "text": "Visit our website" },
    "action": {
      "name": "cta_url",
      "parameters": {
        "display_text": "Open Website",
        "url": "https://example.com"
      }
    }
  }
}
```

### 7) Interactive — Flow Message
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "flow",
    "header": { "type": "text", "text": "Complete Your Profile" },
    "body": { "text": "Please fill in your details below." },
    "footer": { "text": "Powered by The Platform" },
    "action": {
      "name": "flow",
      "parameters": {
        "flow_message_version": "3",
        "flow_token": "flow_1712613581234123",
        "flow_id": "<FLOW_ID>",
        "flow_cta": "Open Form",
        "flow_action": "navigate",
        "flow_action_payload": {
          "screen": "WELCOME",
          "data": {
            "user_name": "John"
          }
        },
        "mode": "published"
      }
    }
  }
}
```
`flow_token` is your session identifier. Generate it uniquely per send (e.g. `flow_{timestamp}{random}`).
`mode`: `"draft"` for testing, `"published"` for production.
`flow_action`: `"navigate"` (opens first screen) or `"data_exchange"` (requires your endpoint).

### 8) Interactive — Call Permission Request
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "call_permission_request",
    "action": { "name": "call_permission_request" },
    "body": { "text": "We'd like to call you to assist with your query." }
  }
}
```
Must be sent before initiating a voice call. User must grant permission.

### 9) Reaction
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "reaction",
  "reaction": {
    "message_id": "wamid.HBgL...",
    "emoji": "👍"
  }
}
```
Send `"emoji": ""` to remove a reaction.

### 10) Mark as Read
```json
{
  "messaging_product": "whatsapp",
  "status": "read",
  "message_id": "wamid.HBgL..."
}
```

### 11) Typing Indicator
```json
{
  "messaging_product": "whatsapp",
  "status": "typing",
  "message_id": "wamid.HBgL..."
}
```
Appears as "typing..." in the user's chat. Use sparingly — only when actively composing.

### 12) Location
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "location",
  "location": {
    "longitude": "-122.143199",
    "latitude": "37.484213",
    "name": "Meta Headquarters",
    "address": "1 Hacker Way, Menlo Park, CA 94025"
  }
}
```

### 13) Contacts
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "contacts",
  "contacts": [
    {
      "name": { "first_name": "John", "formatted_name": "John Doe" },
      "phones": [{ "phone": "+1 (555) 123-4567", "type": "WORK" }],
      "emails": [{ "email": "john@example.com", "type": "WORK" }],
      "urls": [{ "url": "https://example.com", "type": "WORK" }]
    }
  ]
}
```

### 14) Single Product (Commerce)
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "product",
    "body": { "text": "Check out our newest item!" },
    "footer": { "text": "Valid while supplies last" },
    "action": {
      "catalog_id": "<CATALOG_ID>",
      "product_retailer_id": "<PRODUCT_SKU>"
    }
  }
}
```

### 15) Multi-Product (Commerce)
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "product_list",
    "header": { "type": "text", "text": "Our Top Products" },
    "body": { "text": "Browse our selection." },
    "action": {
      "catalog_id": "<CATALOG_ID>",
      "sections": [
        {
          "title": "Category 1",
          "product_items": [
            { "product_retailer_id": "<SKU_1>" },
            { "product_retailer_id": "<SKU_2>" }
          ]
        }
      ]
    }
  }
}
```

### 16) BSUID Routing (WhatsApp Usernames — 2026)
When a user has opted into WhatsApp Usernames, use the `recipient` key instead of `to`:
```json
{
  "messaging_product": "whatsapp",
  "recipient": { "bsuid": "<BSUID_FROM_WEBHOOK>" },
  "type": "text",
  "text": { "body": "Hello!" }
}
```

### 17) Interactive — Media Carousel (NEW)
Send 2–10 horizontally scrollable cards, each with an image or video header, body text, and a CTA button. Only valid inside the CSW (free-form message — not a template).

```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "carousel",
    "body": { "text": "Check out our latest offers!" },
    "action": {
      "cards": [
        {
          "card_index": 0,
          "components": {
            "header": {
              "type": "image",
              "image": { "id": "<MEDIA_ID_1>" }
            },
            "body": { "text": "Exclusive deal #1" },
            "buttons": [
              {
                "type": "cta_url",
                "cta_url": {
                  "display_text": "Shop now",
                  "url": "https://example.com/deal1"
                }
              }
            ]
          }
        },
        {
          "card_index": 1,
          "components": {
            "header": {
              "type": "image",
              "image": { "id": "<MEDIA_ID_2>" }
            },
            "body": { "text": "Exclusive deal #2" },
            "buttons": [
              {
                "type": "cta_url",
                "cta_url": {
                  "display_text": "Shop now",
                  "url": "https://example.com/deal2"
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```

**Carousel constraints**:
- Minimum **2 cards**, maximum **10 cards**.
- All cards must have the same structure (header type, button types).
- `body` field on the outer message is required (max 1024 chars).
- Per-card `body` is optional (max 160 chars, up to 2 line breaks).
- No `header`, `footer`, or `buttons` outside the cards array.
- Button options per card: `cta_url` (URL button) or `quick_reply`.
- For **carousel template messages** (outside CSW), use the carousel template component structure in section N.

### 18) Interactive — Location Request (NEW)
Prompts the user to share their location with a tappable "Send Location" button.

```json
{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "location_request_message",
    "body": {
      "text": "Let's find your nearest store. Please share your location."
    },
    "action": {
      "name": "send_location"
    }
  }
}
```

When the user taps the button and shares their location, the response arrives as a standard `location` type webhook (see Section J item 7).

**Constraints**:
- Can only be sent within an active CSW (free-form message).
- Only `body` is required. No header or footer.
- `action.name` must be exactly `"send_location"`.

### 19) Address Message (NEW)
Prompts the user to enter a delivery or pickup address using a structured UI form. Currently supported in India and select markets.

```json
{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "address_message",
    "body": {
      "text": "Please enter your delivery address."
    },
    "action": {
      "name": "address_message",
      "parameters": {
        "country": "IN"
      }
    }
  }
}
```

The user fills in a structured address form (name, address line, city, state, pin code, phone). Their response arrives via webhook as an `address` message type with the structured data.

**Inbound Address Webhook Payload**:
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "interactive",
  "interactive": {
    "type": "address",
    "address": {
      "name": "John Doe",
      "phone_number": "+91 98765 43210",
      "in_pin_code": "400001",
      "floor_number": "3",
      "building_name": "Horizon Towers",
      "address": "123 Main Street",
      "landmark_area": "Near Central Park",
      "city": "Mumbai",
      "state": "Maharashtra",
      "country": "IN"
    }
  }
}
```

---

## L) Calling (Voice) — Complete Integration Guide

### Overview
WhatsApp Cloud API Calling enables WebRTC-based voice calling between businesses and users. The architecture is:
1. Business sends a **Call Permission Request** interactive message.
2. User grants permission.
3. User or business initiates a call.
4. Business receives a **Connect Webhook** with SDP offer from the user's device.
5. Business sends an **Accept** action with its own SDP answer via the `/calls` endpoint.
6. Call is established (WebRTC P2P or media relay).
7. Business sends **Terminate** action to end the call.
8. Business receives a **Terminate Webhook** with call summary.

### Call Action Endpoint
```
POST /{Version}/{PHONE_NUMBER_ID}/calls
```

### Call Action Payloads

#### Pre-Accept (Signal readiness before full accept)
```json
{
  "messaging_product": "whatsapp",
  "call_id": "<CALL_ID>",
  "action": "pre_accept",
  "biz_opaque_callback_data": "optional-tracking-string-max-512-chars"
}
```

#### Accept Call
```json
{
  "messaging_product": "whatsapp",
  "call_id": "<CALL_ID>",
  "action": "accept",
  "session": {
    "sdp_type": "answer",
    "sdp": "v=0\r\no=- 46117 2 IN IP4 127.0.0.1\r\ns=-\r\nt=0 0\r\n..."
  },
  "biz_opaque_callback_data": "optional-tracking-string"
}
```

#### Reject Call
```json
{
  "messaging_product": "whatsapp",
  "call_id": "<CALL_ID>",
  "action": "reject"
}
```

#### Initiate Outbound Call
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "action": "connect",
  "session": {
    "sdp_type": "offer",
    "sdp": "v=0\r\no=- 46117 2 IN IP4 127.0.0.1\r\ns=-\r\nt=0 0\r\n..."
  },
  "biz_opaque_callback_data": "optional-tracking-string"
}
```

#### Terminate Call
```json
{
  "messaging_product": "whatsapp",
  "call_id": "<CALL_ID>",
  "action": "terminate"
}
```

**Valid action values**: `pre_accept`, `accept`, `reject`, `connect`, `terminate`

### SIP (Session Initiation Protocol) Integration
For businesses wanting to connect WhatsApp Calling to existing telephony/contact-center infrastructure via SIP:
- Meta supports SIP-based integration as an alternative to WebRTC direct integration.
- SIP gateway receives and forwards SIP INVITE messages to/from WhatsApp.
- Reference: `https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip/`

### Calling Webhooks

Subscribe to the `calls` field on your WABA to receive calling events.

#### Connect Webhook (Inbound Call Ready for Pickup)
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "calls",
      "value": {
        "messaging_product": "whatsapp",
        "metadata": {
          "display_phone_number": "15551234567",
          "phone_number_id": "<PHONE_NUMBER_ID>"
        },
        "calls": [{
          "call_id": "<CALL_ID>",
          "from": "<USER_WA_ID>",
          "timestamp": "1669876543",
          "direction": "USER_INITIATED",
          "session": {
            "sdp_type": "offer",
            "sdp": "v=0\r\n..."
          },
          "biz_opaque_callback_data": "your-tracking-string"
        }]
      }
    }]
  }]
}
```

#### Terminate Webhook (Call Ended)
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "<WABA_ID>",
    "changes": [{
      "field": "calls",
      "value": {
        "messaging_product": "whatsapp",
        "calls": [{
          "call_id": "<CALL_ID>",
          "from": "<USER_WA_ID>",
          "timestamp": "1669876600",
          "direction": "USER_INITIATED",
          "duration_seconds": 57,
          "termination_reason": "NORMAL",
          "biz_opaque_callback_data": "your-tracking-string"
        }]
      }
    }]
  }]
}
```

**Direction values** (as of Graph API v21+): `USER_INITIATED`, `BUSINESS_INITIATED` (replaced `INCOMING`/`OUTGOING` in v21, mandatory after Jan 2, 2025 for all versions).

**Termination Reason values**: `NORMAL`, `REJECTED`, `MISSED`, `FAILED`, `BUSY`.

### SDP Requirements
- SDP must comply with **RFC 4566**.
- `sdp_type`: `"offer"` when initiating, `"answer"` when accepting.
- The `connection` field in session is deprecated. Use `session` instead.

### Calling Constraints
- Calling requires specific WABA eligibility from Meta.
- **Not supported in WhatsApp Groups.**
- Calling analytics available via `call_analytics` in the Analytics API.
- Customer Service Window is refreshed when a user calls or accepts a call.
- `biz_opaque_callback_data` max 512 chars. Included in Connect and Terminate webhooks for cross-referencing call sessions.

### Call Settings API
Configure which call scenarios are enabled for your phone number:
```
POST /{Version}/{PHONE_NUMBER_ID}/settings
Body: {
  "calling": {
    "user_initiated": true,
    "business_initiated": true
  }
}
```

---

## M) Flows API — Full Lifecycle

### Flow State Machine
`DRAFT` → `PUBLISHED` → `DEPRECATED` → `DELETED`
(Cannot edit a published flow. Must deprecate before deleting.)

### Flow Creation Sequence

**Step 1: Create Flow**
```
POST /{Version}/{BUSINESS_ID}/flows
Body: { "name": "My Flow", "categories": ["LEAD_GENERATION"] }
Returns: { "id": "<FLOW_ID>" }
```
Valid categories: `SIGN_UP`, `SIGN_IN`, `APPOINTMENT_BOOKING`, `LEAD_GENERATION`, `CONTACT_US`, `CUSTOMER_SUPPORT`, `SURVEY`, `OTHER`.

**Step 2: Upload Flow JSON Asset**
```
POST /{Version}/{FLOW_ID}/assets
Content-Type: multipart/form-data

file: <flow.json content>
name: "flow.json"
asset_type: "FLOW_JSON"
```
The JSON must contain: `version`, `data_api_version`, `routing_model`, `screens[]`.

**Step 3: Publish**
```
POST /{Version}/{FLOW_ID}/publish
```
Once published, JSON cannot be edited.

**Step 4: Deprecate (when sunset)**
```
POST /{Version}/{FLOW_ID}/deprecate
```

**Step 5: Delete**
```
DELETE /{Version}/{FLOW_ID}
```

**Get Flow Asset for Download**
```
GET /{Version}/{FLOW_ID}/assets
→ Find asset with asset_type == "FLOW_JSON"
→ GET the download_url field to retrieve the raw JSON
```

### Data Exchange Mode (Server-Driven Flows)
When `flow_action: "data_exchange"`, WhatsApp calls your endpoint to fetch dynamic screen data. Your endpoint must:
- Be HTTPS.
- Respond within 3 seconds.
- Return a `{ "screen": "NEXT_SCREEN", "data": {...} }` payload.

### Flow Session Tracking (SaaS Rule)
- Key each flow session by `flow_token` (the value you set in the send payload).
- Map `flow_token` → `(thread_id, tenant_id, contact_id, flow_id)`.
- When `nfm_reply` arrives, look up `flow_token` to associate the response with the correct context.

---

## N) Templates — Advanced Reference

### Template Component Types
- `header`: text, image, video, document, location
- `body`: text with variables `{{1}}`, `{{2}}` or named `{{first_name}}`
- `footer`: text only
- `buttons`: up to 10 total buttons of types: `QUICK_REPLY`, `URL`, `PHONE_NUMBER`, `OTP` (auth only), `COPY_CODE`, `MPM` (Multi-Product Message), `CATALOG`

### Template Limits
- Max **250 templates** per WABA if business portfolio is unverified.
- Max **6,000 templates** per WABA if business portfolio is verified AND at least one phone number has an approved display name.
- Max **100 templates created per hour** via API.

### Positional vs. Named Parameters

**Positional** (`{{1}}`, `{{2}}`):
```json
"example": {
  "body_text": [["John", "Doe"]]
}
```

**Named** (`{{first_name}}`, `{{last_name}}`):
```json
"parameter_format": "NAMED",
"example": {
  "body_text_named_params": [
    {"param_name": "first_name", "example": "John"},
    {"param_name": "last_name", "example": "Doe"}
  ]
}
```

### Authentication Template — OTP Buttons
Special `OTPType` values:
- `COPY_CODE` — Shows a "Copy Code" button.
- `ONE_TAP` — Android auto-fill with one tap (requires `PackageName` + `SignatureHash`).
- `ZERO_TAP` — Auto-fills without user tap.

```json
{
  "name": "otp_verification",
  "language": { "code": "en_US" },
  "components": [
    {
      "type": "body",
      "parameters": [
        { "type": "text", "text": "123456" }
      ]
    },
    {
      "type": "button",
      "sub_type": "url",
      "index": "0",
      "parameters": [
        { "type": "text", "text": "123456" }
      ]
    }
  ]
}
```

### Coupon Code Template (NEW)
A specialized marketing template with a built-in "Copy Code" button for promotional coupons.

**Template creation component**:
```json
{
  "type": "BUTTONS",
  "buttons": [{
    "type": "COPY_CODE",
    "example": ["SAVE20"]
  }]
}
```

**Send payload**:
```json
{
  "type": "button",
  "sub_type": "COPY_CODE",
  "index": "0",
  "parameters": [{
    "type": "coupon_code",
    "coupon_code": "SAVE20"
  }]
}
```
- Max coupon code length: **15 characters**.
- Only one `COPY_CODE` button allowed per template.
- Not supported in WhatsApp Web client.
- Only `MARKETING` category.

### Limited-Time Offer (LTO) Template (NEW)
A marketing template type that shows a countdown timer and a coupon code.

**Template creation body** (include `limited_time_offer` object):
```json
{
  "name": "flash_sale_offer",
  "language": "en_US",
  "category": "MARKETING",
  "components": [
    {
      "type": "LIMITED_TIME_OFFER",
      "limited_time_offer": {
        "text": "Offer expires soon",
        "has_expiration": true
      }
    },
    {
      "type": "BUTTONS",
      "buttons": [
        {
          "type": "COPY_CODE",
          "example": ["FLASH50"]
        },
        {
          "type": "URL",
          "text": "Shop Now",
          "url": "https://shop.example.com/{{1}}"
        }
      ]
    }
  ]
}
```

**Send payload** (with expiration timestamp):
```json
{
  "type": "limited_time_offer",
  "parameters": [{
    "type": "limited_time_offer",
    "limited_time_offer": {
      "expiration_time_ms": 1700000000000
    }
  }]
}
```
- If `has_expiration: true`, a `COPY_CODE` button is required and must appear first; a URL button is also required.
- Footer components are not supported.
- Only `MARKETING` category.

### Media Card Carousel Template (NEW)
A template with 2–10 scrollable cards. Can be sent outside the CSW (as a template). Used for product showcases, personalized offers, or multi-item promotions.

**Send payload**:
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "template",
  "template": {
    "name": "product_carousel_promo",
    "language": { "code": "en_US" },
    "components": [
      {
        "type": "body",
        "parameters": [
          { "type": "text", "text": "John" }
        ]
      },
      {
        "type": "carousel",
        "cards": [
          {
            "card_index": 0,
            "components": [
              {
                "type": "header",
                "parameters": [{ "type": "image", "image": { "id": "<MEDIA_ID_1>" } }]
              },
              {
                "type": "body",
                "parameters": [{ "type": "text", "text": "Product A" }]
              },
              {
                "type": "button",
                "sub_type": "quick_reply",
                "index": "0",
                "parameters": [{ "type": "payload", "payload": "card_0_reply" }]
              },
              {
                "type": "button",
                "sub_type": "url",
                "index": "1",
                "parameters": [{ "type": "text", "text": "product-a" }]
              }
            ]
          },
          {
            "card_index": 1,
            "components": [
              {
                "type": "header",
                "parameters": [{ "type": "image", "image": { "id": "<MEDIA_ID_2>" } }]
              },
              {
                "type": "body",
                "parameters": [{ "type": "text", "text": "Product B" }]
              },
              {
                "type": "button",
                "sub_type": "quick_reply",
                "index": "0",
                "parameters": [{ "type": "payload", "payload": "card_1_reply" }]
              },
              {
                "type": "button",
                "sub_type": "url",
                "index": "1",
                "parameters": [{ "type": "text", "text": "product-b" }]
              }
            ]
          }
        ]
      }
    ]
  }
}
```

### Template TTL (Time-to-Live) Customization (NEW)

Meta retries undelivered messages for a TTL period. You can customize this per template type:

| Template Category | Default TTL | Customizable Range |
|---|---|---|
| Authentication | 10 minutes | 30 seconds – 15 minutes |
| Utility | 30 days | 30 seconds – 12 hours |
| Marketing (MM API only) | 30 days | 12 hours – 30 days |
| All others | 30 days | Not customizable |

**Set TTL when creating a template**:
```json
{
  "name": "otp_login",
  "category": "AUTHENTICATION",
  "language": "en_US",
  "ttl": 300,
  "components": [...]
}
```
`ttl` is in **seconds**. Messages not delivered within TTL are silently dropped — no webhook is fired after TTL expires.

**SaaS Rule**: If no `delivered` webhook arrives within the TTL window, assume the message was dropped. Add this to your campaign dead-letter handling.

### Template Quality & Throttling
- Templates with high negative feedback are automatically paused by Meta.
- You receive `message_template_status_update` webhook with `event: "PAUSED"`.
- Continued poor performance → `event: "DISABLED"`.
- Monitor template analytics via `/{BusinessID}/template_analytics` for delivered, read, and click rates.
- Watch `message_template_quality_update` for quality score changes before they escalate to pauses.

### Template Library (NEW)
Meta provides a library of pre-approved templates that businesses can use immediately (skipping the review queue). Access via:
- WhatsApp Manager UI: Account Tools → Message Templates → Template Library.
- API: `GET /{Version}/{WABA_ID}/message_templates?category=LIBRARY`

Library templates cover common categories: order confirmations, shipping updates, appointment reminders, OTPs, etc.

### Template Sync Strategy (for The Platform)
On app startup and every 15 minutes (or on `message_template_status_update` / `message_template_quality_update` / `template_category_update` webhooks):
```
GET /{Version}/{WABA_ID}/message_templates
  ?fields=id,name,status,language,category,quality_score,components
  &limit=100
```
Page through results (check `paging.cursors.after`). Store in your DB. Serve from DB for UI.

---

## O) Analytics API

### Messaging Analytics
```
GET /{Version}/{BusinessID}?fields=analytics.start(1660000000).end(1660100000).granularity(DAY)
```

| Category | What it Tracks |
|---|---|
| `analytics` | Messages sent, delivered counts per phone number |
| `pricing_analytics` | Costs broken down by category, tier, type, country using `dimensions(COUNTRY,CATEGORY,TYPE)` |
| `template_analytics` | Per-template delivered, read, click, cost (use `/{BusinessID}/template_analytics`) |
| `call_analytics` | Call counts, average durations, costs |

### Template Analytics Endpoint
```
GET /{Version}/{BusinessID}/template_analytics
  ?start=1660000000
  &end=1660100000
  &granularity=DAY
  &metric_types=SENT,DELIVERED,READ,CLICKED
  &template_ids=<ID1>,<ID2>
```

---

## P) Phone Number Registration Workflow

```
1. POST /{PHONE_NUMBER_ID}/request_code
   Body: { "code_method": "SMS", "language": "en" }

2. POST /{PHONE_NUMBER_ID}/verify_code
   Body: { "code": "123456" }

3. POST /{PHONE_NUMBER_ID}/register
   Body: { "messaging_product": "whatsapp", "pin": "123456" }

4. (Optional) POST /{PHONE_NUMBER_ID}/deregister
   → Removes number from Cloud API. Requires re-registration to send again.
```

---

## Q) QR Codes & Short Links

### Create QR Code
```
POST /{PHONE_NUMBER_ID}/qr_codes
Body: {
  "prefilled_message": "I'm interested in your product",
  "generate_qr_image": "svg"
}
Returns: { "code": "<QR_CODE_ID>", "prefilled_message": "...", "deep_link_url": "https://wa.me/...", "qr_image_url": "..." }
```

### List QR Codes
```
GET /{PHONE_NUMBER_ID}/qr_codes
```

### Delete QR Code
```
DELETE /{PHONE_NUMBER_ID}/qr_codes/{QR_CODE_ID}
```

**Constraints**:
- Per-number QR code limits exist.
- Deep link format: `https://wa.me/{PHONE_NUMBER}?text=<URL-encoded-message>`

---

## R) Commerce & Catalogs

### Catalog Setup
1. Create catalog in Commerce Manager or via API: `POST /{BusinessID}/owned_product_catalogs`
2. Connect catalog to WABA in WhatsApp Manager.
3. Add products: `POST /{CATALOG_ID}/products`

### Product Input Rules
- Price must be an integer string of **cents** (e.g. `"1000"` for $10.00), NOT a float.
- Always pair with a `currency` field (e.g. `"USD"`).

### Product Card Carousel (Catalog) — NEW
Send catalog product cards in a horizontally scrollable carousel. Distinct from the media carousel — this pulls data from your Meta catalog.

```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "product_list",
    "header": { "type": "text", "text": "Today's Picks" },
    "body": { "text": "Swipe to browse our featured products." },
    "action": {
      "catalog_id": "<CATALOG_ID>",
      "sections": [
        {
          "title": "Featured",
          "product_items": [
            { "product_retailer_id": "<SKU_1>" },
            { "product_retailer_id": "<SKU_2>" },
            { "product_retailer_id": "<SKU_3>" }
          ]
        }
      ]
    }
  }
}
```

### Catalog Link Message
Share a link to your full catalog storefront:
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "catalog_message",
    "body": { "text": "Browse our full catalog!" },
    "action": {
      "name": "catalog_message",
      "parameters": {
        "thumbnail_product_retailer_id": "<SKU_FOR_THUMBNAIL>"
      }
    }
  }
}
```

### Commerce Webhook Events
- Incoming `type: "order"` — user submitted a cart (see payload in Section J).
- Parse `product_items[]` for SKU, quantity, price.
- Send an order confirmation via Utility template (chargeable unless in CSW).

---

## S) Groups

### Eligibility
- Requires Official Business Account (OBA) status.
- Not all WABAs are eligible. Must request access from Meta.

### Key Constraints
- Max **256 participants** per group (including the business).
- Calling NOT supported in groups.
- Group messages share the same `/{PHONE_NUMBER_ID}/messages` endpoint with the group ID as recipient.

### Group Management — Full API Reference

#### Create Group
```
POST /{Version}/{WABA_ID}/groups
Body: {
  "messaging_product": "whatsapp",
  "subject": "My Business Group",
  "icon": "<MEDIA_ID>"
}
Returns: { "id": "<GROUP_ID>" }
```

#### Get Group Info
```
GET /{Version}/{GROUP_ID}
?fields=id,subject,description,creation_time,participants,icon
```

#### Update Group
```
POST /{Version}/{GROUP_ID}
Body: {
  "messaging_product": "whatsapp",
  "subject": "Updated Group Name",
  "description": "Updated description"
}
```

#### Add / Remove Participants
```
POST /{Version}/{GROUP_ID}/participants
Body: {
  "messaging_product": "whatsapp",
  "action": "add",
  "users": [
    { "wa_id": "<USER_WA_ID_1>" },
    { "wa_id": "<USER_WA_ID_2>" }
  ]
}
```
Use `"action": "remove"` to remove participants (admin only).

#### Get Invite Link
```
GET /{Version}/{GROUP_ID}/invite_link
Returns: { "invite_link": "https://chat.whatsapp.com/..." }
```

#### Reset Invite Link
```
DELETE /{Version}/{GROUP_ID}/invite_link
```
Invalidates current link and generates a new one.

#### List Groups
```
GET /{Version}/{WABA_ID}/groups
```

#### Sending Messages to a Group
```json
{
  "messaging_product": "whatsapp",
  "to": "<GROUP_ID>",
  "type": "text",
  "text": { "body": "Hello group!" }
}
```
The `to` field is the Group ID, not a phone number.

#### Group Message Webhooks
Group messages arrive with a `group` context:
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "text",
  "text": { "body": "Hello!" },
  "context": {
    "group_id": "<GROUP_ID>"
  }
}
```
Subscribe to the `messages` field as normal. Group messages include a `context.group_id`.

Official: `https://developers.facebook.com/documentation/business-messaging/whatsapp/groups`

---

## T) SaaS Integration Rules (The Platform-Specific)

### Tenant Isolation (Hard Requirement)
Every webhook and every outbound call must resolve to a single tenant context using:
```
WABA_ID + PHONE_NUMBER_ID → (org_id, account_id, tenant_config)
```
If the mapping fails → log the webhook as unroutable + alert.

### Idempotency Matrix

| Layer | De-dupe Key | TTL |
|---|---|---|
| Inbound messages | `(PHONE_NUMBER_ID, WA_MESSAGE_ID)` | 48 hours |
| Inbound status updates | `(PHONE_NUMBER_ID, WA_MESSAGE_ID, status)` | 48 hours |
| Outbound sends | `X-Idempotency-Key` header (caller-supplied) | 24 hours |
| Flow sessions | `flow_token` | Until flow completed/expired |
| MM API campaigns | Campaign ID + recipient WA_ID | Per-campaign TTL |

### Security Rules
- Tokens: server-only, encrypted at rest, never in logs, never in frontend state.
- Webhook signatures: validate every POST before processing.
- Media URLs: never leaked to frontend. Always serve via your own CDN.
- `biz_opaque_callback_data`: validate it came from your system before trusting it.
- Marketing opt-out: honor `user_preferences` webhook with `marketing_stop: true` immediately.

### Retry Strategy by Error Type

| Scenario | Strategy |
|---|---|
| `130429` (throughput) | Exponential backoff + jitter. Queue and drain at safe rate. |
| `4` (API rate limit) | Back off. Queue and drain within hourly window. |
| `131026` (undeliverable) | Mark contact as potentially invalid. Do not retry same message. |
| `131047` (outside CSW) | Block send. Require template selection from UI. |
| `131000` (unknown) | Retry up to 3x with backoff. Escalate to Ops if persistent. |
| `190` (token expired) | Immediately rotate token. Alert ops. Do not retry until resolved. |
| `368` (policy block) | Do not retry. Alert tenant. Require appeal flow. |
| `1`, `2`, `131016` | Check platform status. Retry after 5 min. |
| `132015`, `132016` (template paused/disabled) | Stop sending that template. Alert tenant. |
| `131049` (Meta chose not to deliver) | Do not retry. Review send patterns. |
| `131060` (Coexistence transient) | Wait 5s. Retry once. |

### Minimum Webhook Persistence Schema
```sql
webhook_events (
  id            UUID PRIMARY KEY,
  received_at   TIMESTAMPTZ NOT NULL,
  waba_id       TEXT NOT NULL,
  phone_num_id  TEXT NOT NULL,
  event_type    TEXT NOT NULL,  -- 'message' | 'status' | 'template_status' | 'quality' | 'call' | 'user_preferences' | etc.
  wa_message_id TEXT,
  dedup_key     TEXT UNIQUE,
  status        TEXT NOT NULL DEFAULT 'pending', -- 'processed' | 'ignored' | 'failed'
  fbtrace_id    TEXT,
  raw_payload   JSONB NOT NULL,
  processed_at  TIMESTAMPTZ,
  error_detail  TEXT
)
```

---

## U) Limits, Rate Limits, and Ops Reference

### API Rate Limits Summary

| Limit Type | Default | Notes |
|---|---|---|
| Management API calls | 200 req/hr/app/WABA | Unlinked apps |
| Management API calls (linked) | 5,000 req/hr/app/WABA | Linked + at least 1 registered number |
| Message throughput | 80 mps | Per phone number. Can increase for eligible accounts. |
| Message throughput (eligible) | 1,000 mps | Auto-upgraded for high-volume accounts. |
| Registration attempts | 10 per 72 hours | Per phone number. |
| Template creation | 100 per hour | Per WABA. |

### Platform Status
Monitor: `https://metastatus.com/whatsapp-business-api`
Also: Meta Business Help Center → Support → Platform Status

### Versioning Policy
- Pin API version in all outbound calls. Example: `v21.0`.
- Only bump versions as a planned, tested release.
- Breaking changes (like direction field values in calling) are announced in Meta changelogs.
- Test version bumps against: webhooks, send, templates, media, flows, calling, catalogs, groups.

---

## V) 2026 Platform Changes — Action Items for The Platform

| Change | Status | The Platform Action Required |
|---|---|---|
| Pricing: CBP → PMP | **LIVE (July 1, 2025)** | Parse `pricing.pricing_model: "PMP"` in status webhooks. Update billing engine. |
| Service conversations free (no cap) | **LIVE (Nov 1, 2024)** | Remove 1,000/month free limit from billing logic. All service = free. |
| Messaging tiers simplified (2K/10K removed → 100K) | **Rolling Q1–Q2 2026** | Update tier UI. Remove 2K/10K display. Show 100K as default post-verification. |
| Portfolio pacing for campaigns | **Rolling 2026** | Add "paused by Meta" campaign state. Build campaign delivery monitoring. |
| WhatsApp Usernames + BSUID | **Rolling 2026** | Extend contact model for BSUID. Support `recipient.bsuid` in outbound. Never assume `wa_id` is a phone number. |
| Direction field in calls webhook (v21+) | **LIVE** | Use `USER_INITIATED`/`BUSINESS_INITIATED` not `INCOMING`/`OUTGOING`. |
| `authentication-international` category | **LIVE** | Parse new pricing category. Display to tenants for India/LATAM OTP cost visibility. |
| `messaging_limit_tier` field deprecated | **LIVE** | Use `whatsapp_business_manager_messaging_limit` field instead. |
| Marketing Messages (MM) API — GA | **LIVE (April 2025)** | Implement `/marketing_messages` endpoint. Support `marketing_lite` pricing category in status webhooks. Show MM API option in campaign creation UI. |
| US marketing pause | **LIVE (April 1, 2025)** | Block marketing template sends to +1 numbers. Surface error in UI. |
| Template TTL customization | **LIVE** | Expose TTL setting in template creation UI. Handle message drop without webhook at TTL expiry. |
| `user_preferences` webhook (opt-out) | **LIVE** | Subscribe to field. Update contact opt-in/opt-out status on receipt. Block marketing to opted-out users. |
| `template_category_update` webhook | **LIVE** | Subscribe to field. Trigger template sync on receipt. Alert tenant on category change (billing impact). |
| Template limit increase (6,000 for verified) | **LIVE** | Update template count limits in UI/backend for verified tenants. |
| Coupon code & LTO templates | **LIVE** | Add template type options in template builder UI. |
| Media card carousel templates | **LIVE** | Add carousel template builder in The Platform. Support carousel component send payload. |
| Interactive media carousel messages | **LIVE** | Implement carousel message type in composer. |
| Address messages | **LIVE (India + select)** | Add address message type for India tenants. Handle inbound `interactive.address` webhook. |
| Location request messages | **LIVE** | Add location request button to message composer. |
| Conversational Components API | **LIVE** | Expose welcome message / ice breaker / commands settings per phone number in tenant dashboard. |
| Groups API (full management) | **LIVE (OBA only)** | Implement full group management: create, update, add/remove participants, invite links. |
| SIP Calling integration | **LIVE** | Expose SIP config option for enterprise tenants with existing telephony. |
| On-Premises API sunset | **COMPLETE (Oct 2025)** | Remove all on-premises code paths. All tenants on Cloud API. `platform_type` = `CLOUD_API` always. |
| Brazil Payments (PIX, Boleto, One-Click) | **LIVE (Brazil)** | Implement Brazil payment methods for Brazil tenants. |
| Local currency billing (Mexico Jan 2026+) | **Rolling 2026** | Update billing display for local currency markets. |
| Pre-Verified Phone Numbers API | **LIVE** | Use for frictionless Embedded Signup flows where phone numbers are pre-verified by partner. |
| WABA Activities API | **LIVE** | Use for compliance audit logging of WABA management actions. |

---

## W) Payments API — India & Brazil (In-Chat Checkout)

### Overview
WhatsApp Payments enables businesses to collect payments directly inside a WhatsApp chat. Currently supported in **India** (UPI, credit/debit cards, net banking) and **Brazil** (debit/credit cards, PIX, Boleto, bank methods). Both markets require eligibility approval from Meta before use.

Official docs:
- India: `https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/overview/`
- Brazil: `https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/overview/`

### Payment Flow Architecture

```
1. Customer browses catalog / sends order intent
2. Business sends → order_details interactive message (invoice + pay button)
3. Customer taps "Review and Pay" → chooses payment method inside WhatsApp
4. Payment Gateway processes transaction → sends webhook to Business
5. Business sends → order_status interactive message (confirms outcome to customer)
6. Business also calls Payment Lookup API to cross-verify status (security requirement)
```

**Never rely solely on Payment Gateway webhooks for order fulfillment. Always verify via the Payment Lookup API.**

### Country-Specific Methods

| Country | Supported Methods | Transaction Limits |
|---|---|---|
| India | UPI (all UPI apps), debit/credit cards, net banking | ₹1,00,000 per transaction; daily caps apply |
| Brazil | Debit/credit cards, PIX, Boleto, supported bank methods via payment partners | R$1,000 per transaction; monthly caps apply |

India integrations require linking an approved payment gateway (Razorpay, PayU, Cashfree, Billdesk, CCAvenue) to your WhatsApp Business Manager account.

### Setup Prerequisites (India)
1. Business must be registered and operating in India.
2. WhatsApp Business Account must be in High or Medium quality tier.
3. Link an approved Payment Gateway in Meta Business Manager → Payment Gateway page.
4. Obtain `payment_configuration` token from the linked gateway.

### Send `order_details` Message (Invoice / Pay Now)
```json
{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "order_details",
    "header": {
      "type": "image",
      "image": { "link": "https://your-cdn.com/product.jpg" }
    },
    "body": {
      "text": "Your order is ready! Tap Review and Pay to complete your purchase."
    },
    "footer": {
      "text": "Thank you for shopping with us."
    },
    "action": {
      "name": "review_and_pay",
      "parameters": {
        "reference_id": "ORD-20250401-001",
        "type": "digital-goods",
        "payment_type": "upi",
        "payment_configuration": "<PG_CONFIG_TOKEN>",
        "currency": "INR",
        "total_amount": {
          "value": 49900,
          "offset": 100
        },
        "order": {
          "status": "pending",
          "expiration": {
            "timestamp": "1769000000",
            "description": "Offer expires in 30 minutes"
          },
          "items": [
            {
              "retailer_id": "SKU-BREAD-001",
              "name": "Sourdough Bread",
              "amount": {
                "value": 45000,
                "offset": 100
              },
              "quantity": 2,
              "sale_amount": {
                "value": 40000,
                "offset": 100
              }
            }
          ],
          "subtotal": {
            "value": 45000,
            "offset": 100
          },
          "tax": {
            "value": 2700,
            "offset": 100,
            "description": "GST 6%"
          },
          "shipping": {
            "value": 2200,
            "offset": 100,
            "description": "Standard Delivery"
          },
          "discount": {
            "value": 0,
            "offset": 100,
            "description": ""
          }
        }
      }
    }
  }
}
```

**Amount encoding rule**: All monetary values use `offset: 100`, meaning the `value` field is in the **smallest currency unit × 100**. `value: 49900` with `offset: 100` = ₹499.00.

### Brazil-Specific Payment Methods (NEW)

#### PIX (Brazil)
```json
{
  "action": {
    "name": "review_and_pay",
    "parameters": {
      "reference_id": "ORD-BR-001",
      "type": "physical-goods",
      "payment_type": "pix",
      "payment_configuration": "<PG_CONFIG_TOKEN>",
      "currency": "BRL",
      "total_amount": { "value": 5000, "offset": 100 },
      "order": { "status": "pending", "items": [...] }
    }
  }
}
```

#### Boleto (Brazil)
```json
{
  "action": {
    "name": "review_and_pay",
    "parameters": {
      "payment_type": "boleto",
      "currency": "BRL",
      ...
    }
  }
}
```

#### One-Click Payments (Brazil)
Meta supports one-click payment flows for returning customers who have saved payment methods. Reference: `https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/one-click-payments/`

### Send `order_status` Message (After Payment Confirmed/Failed)
```json
{
  "messaging_product": "whatsapp",
  "recipient_type": "individual",
  "to": "<USER_WA_ID>",
  "type": "interactive",
  "interactive": {
    "type": "order_status",
    "body": {
      "text": "Your payment was successful! We're preparing your order."
    },
    "action": {
      "name": "review_order",
      "parameters": {
        "reference_id": "ORD-20250401-001",
        "order": {
          "status": "captured",
          "description": "Payment of ₹499.00 received.",
          "label": "Order Confirmed"
        }
      }
    }
  }
}
```

**`order.status`** values: `"pending"`, `"captured"`, `"failed"`.

### Payment Lookup API (Security Verification)
```
GET /{Version}/{WABA_ID}/payments/{reference_id}
Authorization: Bearer <ACCESS_TOKEN>
```
Returns the current payment status. Always call before order fulfillment.

### SaaS Rules for Payments
- `reference_id` must be globally unique across your platform (not just per tenant). Use ULIDs or UUIDs.
- Store `reference_id` → `(tenant_id, order_id, user_wa_id, amount)` before sending `order_details`.
- Never fulfill orders based on gateway webhooks alone. Cross-verify via Payment Lookup API.
- Payment conversations open a user-initiated CSW. All messages within that CSW are **free service messages**.
- Monitor `payment_configuration_update` webhook to detect gateway config deactivation.

---

## X) Webhook Fields — Complete Subscribable Fields Reference

When registering webhooks in the Meta App Dashboard, you subscribe to **fields** on the `whatsapp_business_account` object.

### All Subscribable Webhook Fields

| Field | Trigger | Permissions Required |
|---|---|---|
| `messages` | All inbound messages + outbound delivery/read statuses | `whatsapp_business_messaging` |
| `message_template_status_update` | Template approved / rejected / paused / disabled | `whatsapp_business_management` |
| `message_template_quality_update` | Template quality score changed (GREEN/YELLOW/RED) | `whatsapp_business_management` |
| `template_category_update` | Meta re-categorized a template | `whatsapp_business_management` |
| `phone_number_quality_update` | Phone number quality rating changed (FLAGGED/UNFLAGGED) | `whatsapp_business_management` |
| `phone_number_name_update` | Display name approved or rejected after name change request | `whatsapp_business_management` |
| `account_update` | WABA account state changes (restriction, ban, partner events) | `whatsapp_business_management` |
| `account_alerts` | WABA health alerts (bans, restrictions, policy violations) | `whatsapp_business_management` |
| `account_review_update` | WABA review decision (APPROVED / REJECTED) | `whatsapp_business_management` |
| `business_capability_update` | Messaging tier limits changed, max phone numbers changed | `whatsapp_business_management` |
| `security` | Security alerts for the account | `whatsapp_business_management` |
| `flows` | Flow endpoint availability notifications (for data_exchange mode) | `whatsapp_business_management` |
| `calls` | Call lifecycle events (connect, terminate) | `whatsapp_business_messaging` (+ calling eligibility) |
| `user_preferences` | User opted out of marketing messages | `whatsapp_business_messaging` |
| `payment_configuration_update` | Payment gateway config activated / deactivated | `whatsapp_business_management` |
| `history` | Past message history (Coexistence / SMB onboarding) | Solution Provider only |
| `smb_app_state_sync` | Business customer's contacts sync state (Coexistence) | Solution Provider only |
| `smb_message_echoes` | Messages sent from WhatsApp Business App side in Coexistence | Solution Provider only |

**Note**: Fields marked "Solution Provider only" require App Review approval for advanced access.

### Webhook Delivery Guarantees & Edge Cases

| Scenario | Behavior |
|---|---|
| Your endpoint returns non-200 | Meta retries with decreasing frequency for up to **7 days** |
| Response timeout | 5 second hard limit. Return 200 immediately and process async. |
| Payload size | Max **3 MB** per webhook payload |
| Same message on multiple devices | Can trigger both `delivered` and `failed` for same `wamid`. Both are valid — delivered = reached at least one device. |
| API version upgrade | Up to **5 min downtime** during Meta's update. Design for transient failures. |

### Webhook Subscription Management via API
```
# Subscribe a WABA to a webhook field
POST /{Version}/{WABA_ID}/subscribed_apps
Body: { }

# List subscribed apps
GET /{Version}/{WABA_ID}/subscribed_apps

# Unsubscribe
DELETE /{Version}/{WABA_ID}/subscribed_apps
```

### Webhook Subscription for Phone Number Level
```
# Set phone number webhook (overrides WABA webhook)
POST /{Version}/{PHONE_NUMBER_ID}
Body: {
  "webhook_configuration": {
    "endpoint": "https://your-endpoint.com/webhook"
  }
}
```

---

## Y) WABA Management API — Account Lifecycle

### WABA Fields Query
```
GET /{Version}/{WABA_ID}?fields=name,status,currency,country,business_verification_status,account_review_status,on_behalf_of_business_info,primary_funding_id
```

**Key fields returned**:
- `name` — Display name of the WABA
- `status` — `ACTIVE`, `FLAGGED`, `RESTRICTED`, `BANNED`, `PENDING_DELETION`
- `currency` — Billing currency (cannot be changed once credit line attached)
- `country` — Country code (cannot be changed once credit line attached)
- `business_verification_status` — `verified`, `not_verified`, `pending`

### WABA Limits
- Max **250 message templates** per WABA (unverified portfolio).
- Max **6,000 message templates** per WABA (verified portfolio + approved display name).
- Max **20 WABAs** per Meta Business Account (by default).
- Max **2 registered phone numbers** per Business Account initially (can be increased to 20).

### WABA Activities API (NEW)
Retrieve an audit log of management actions taken on a WABA:
```
GET /{Version}/{WABA_ID}/activities
  ?start_time=1660000000
  &end_time=1660100000
  &event_types=TEMPLATE_CREATED,TEMPLATE_DELETED,PHONE_NUMBER_REGISTERED
```
Use for compliance reporting and tenant audit trails.

### Schedules API (NEW)
Schedule future message sends at the WABA level:
```
POST /{Version}/{WABA_ID}/scheduled_messages
Body: {
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "scheduled_timestamp": 1700000000,
  "type": "template",
  "template": { ... }
}
```
List scheduled messages:
```
GET /{Version}/{WABA_ID}/scheduled_messages
```
Cancel a scheduled message:
```
DELETE /{Version}/{SCHEDULED_MESSAGE_ID}
```

### Phone Number Management

**List all phone numbers on a WABA**:
```
GET /{Version}/{WABA_ID}/phone_numbers
```

**Get phone number details**:
```
GET /{Version}/{PHONE_NUMBER_ID}
?fields=verified_name,code_verification_status,display_phone_number,quality_rating,platform_type,throughput,last_onboarded_time
```

**Key phone number fields**:
- `quality_rating` — `GREEN`, `YELLOW`, `RED`
- `platform_type` — `CLOUD_API` (only valid value post-Oct 2025)
- `throughput.level` — `STANDARD` (80 mps) or `HIGH` (1000 mps)
- `code_verification_status` — `VERIFIED`, `NOT_VERIFIED`

---

## Z) Block Users API

WhatsApp Cloud API supports programmatically blocking users from messaging your business number.

### Block a User
```
POST /{Version}/{PHONE_NUMBER_ID}/block_users
Body: {
  "messaging_product": "whatsapp",
  "block_users": [
    { "user": "<USER_WA_ID>" }
  ]
}
```

### Unblock a User
```
DELETE /{Version}/{PHONE_NUMBER_ID}/block_users
Body: {
  "messaging_product": "whatsapp",
  "block_users": [
    { "user": "<USER_WA_ID>" }
  ]
}
```

### List Blocked Users
```
GET /{Version}/{PHONE_NUMBER_ID}/block_users
```

**SaaS Rules for Block Users**:
- Surface as a per-contact action in your inbox UI.
- Blocked users cannot send messages to that phone number.
- Store block state in your tenant's contact record.
- Block/unblock is phone-number-scoped, not WABA-scoped.

---

## AA) Embedded Signup — Tech Provider / Solution Provider Integration

The Platform is a Tech Provider (BSP-style). Customers onboard their WABAs via Meta's **Embedded Signup** flow.

Official docs: `https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview/`

### Embedded Signup Versions
- **v4** (current, recommended): `https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4/`
- v4 adds: pre-verified phone number support, app-only install mode, hosted ES option.

### Embedded Signup Flow (OAuth)
1. Embed Meta's JavaScript SDK in your onboarding UI.
2. Customer clicks "Connect WhatsApp" → Meta OAuth popup opens.
3. Customer logs in, selects/creates Business Portfolio, creates/selects WABA, verifies phone number.
4. On success, your app receives an authorization code.
5. Exchange code for a **Business Integration System User Access Token**:
```
POST https://graph.facebook.com/{Version}/oauth/access_token
  ?client_id={APP_ID}
  &client_secret={APP_SECRET}
  &code={AUTH_CODE}
```
6. Use the returned token to subscribe webhooks, send messages, and manage the onboarded WABA.

### Pre-Verified Phone Numbers (NEW)
Partners can pre-verify phone numbers on behalf of customers, bypassing the OTP step in Embedded Signup:
```
POST /{Version}/{BUSINESS_ID}/whatsapp_business_pre_verified_phone_numbers
Body: {
  "phone_number": "+15551234567",
  "phone_number_type": "MOBILE"
}
Returns: { "id": "<PRE_VERIFIED_PHONE_ID>" }
```
Then share this ID with the customer's WABA during Embedded Signup to skip phone verification.

Official: `https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/pre-verified-numbers/`

### Required App Permissions for Embedded Signup
- `whatsapp_business_management`
- `whatsapp_business_messaging`
- `business_management`

### Webhook Setup After Embedded Signup
After receiving the token, immediately:
```
POST /{Version}/{WABA_ID}/subscribed_apps
Authorization: Bearer <SYSTEM_USER_TOKEN>
```
This subscribes your app to receive webhooks for that WABA.

### Multi-Partner Solutions (NEW)
A WABA can be shared across multiple Tech Providers simultaneously:
- Each partner gets scoped access to the WABA.
- Use the Multi-Partner Solutions API to manage partner relationships.
- Reference: `https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions/`

**Create a Multi-Partner Solution**:
```
POST /{Version}/{WABA_ID}/solutions
Body: {
  "partner_app_id": "<PARTNER_APP_ID>",
  "partner_type": "TECH_PROVIDER"
}
```

**List partners on a WABA**:
```
GET /{Version}/{WABA_ID}/solutions
```

### Coexistence (WhatsApp Business App + Cloud API on Same Number)
Meta supports running the WhatsApp Business App and Cloud API on the same phone number simultaneously (Coexistence).

**Coexistence Rules**:
- Messages sent via the Business App trigger `smb_message_echoes` webhook — you must mirror these in your inbox.
- Messages sent via Cloud API appear in the Business App as well.
- Cannot use `/{PHONE_NUMBER_ID}/deregister` to remove a coexistence number. Customer must disconnect from within the Business App.
- `account_update` with `PARTNER_REMOVED` fires when customer disconnects.
- Companion devices: up to 4 supported.

### Token Scoping for Solution Providers
When you receive a Business Integration System User token, it is scoped to the customer's WABA. Store it encrypted, scoped to that tenant. Never mix tokens across tenants.

---

## AB) Opt-In Management

Meta requires that businesses collect opt-in from users before initiating business-initiated conversations.

### Opt-In Requirements
- Must be explicitly collected outside of WhatsApp (website form, SMS, app, etc.).
- Must clearly state that the user is opting into WhatsApp messages.
- Must specify the types of messages they'll receive.
- Must allow easy opt-out at any time.

### Opt-Out Handling
- Honor `user_preferences` webhook with `marketing_stop: true` immediately.
- Honor explicit opt-out requests (user replies "STOP" or similar) → add to suppression list.
- Do not send marketing templates to opted-out users.
- Violation of opt-out signals leads to quality rating drops and account restrictions.

### SaaS Implementation for The Platform
- Per-tenant contact opt-in status field: `opted_in: boolean`, `opted_out_at: timestamp`, `marketing_stop: boolean`.
- Block all business-initiated template sends to opted-out contacts.
- Block marketing templates to contacts where `marketing_stop: true`.
- Provide a UI for tenants to manage opt-in lists.
- Log opt-in collection method and timestamp for compliance audit.

---

## AC) Conversions API for Business Messaging (CAPI)

Meta's Conversions API (CAPI) lets you report purchase/lead-gen signals from WhatsApp conversations back to Meta's ad system. This improves Click-to-WhatsApp ad attribution and lowers effective CPC for your tenants.

Official docs: `https://developers.facebook.com/docs/marketing-api/conversions-api/`

### When to Use
- Tenant runs Click-to-WhatsApp ads.
- User comes in via `referral` webhook (ad attribution) → makes a purchase or generates a lead.
- You report the conversion event to Meta to close the attribution loop.

### Conversion Event Send
```
POST https://graph.facebook.com/{Version}/{PIXEL_ID}/events
```

```json
{
  "data": [
    {
      "event_name": "Purchase",
      "event_time": 1669876545,
      "action_source": "business_messaging",
      "messaging_channel": "whatsapp",
      "user_data": {
        "phone": "<SHA256_HASHED_PHONE>",
        "client_ip_address": "203.0.113.0",
        "client_user_agent": "WhatsApp/2.23.1.75",
        "em": "<SHA256_HASHED_EMAIL>"
      },
      "custom_data": {
        "currency": "INR",
        "value": 499.00,
        "order_id": "ORD-20250401-001",
        "contents": [
          { "id": "SKU-BREAD-001", "quantity": 2 }
        ]
      },
      "event_id": "unique-dedup-event-id"
    }
  ],
  "test_event_code": "TEST12345"
}
```

**Hashing rule**: SHA-256 hash all PII (phone, email) before sending. Phone must include country code, no spaces or dashes before hashing (e.g. `+15551234567` → hash).

**`event_id`**: Use a unique string to de-duplicate events in case of retries. Set the same `event_id` on browser Pixel events and CAPI events for the same conversion to deduplicate.

### Supported Event Names
- `Purchase` — completed sale
- `Lead` — lead form submitted / conversation qualified
- `InitiateCheckout` — user tapped "Review and Pay"
- `AddToCart` — user added product to WhatsApp cart
- `ViewContent` — user viewed a product message
- `CompleteRegistration` — user submitted a WhatsApp Flow registration form

### Connecting CAPI to MM API
When using MM API for campaigns, connect your Meta Pixel to track which campaigns drove conversions:
```
POST /{Version}/{PHONE_NUMBER_ID}/marketing_messages
Body: {
  ...
  "tracking_data": {
    "pixel_id": "<PIXEL_ID>",
    "click_id": "<CLICK_ID_FROM_UTM>"
  }
}
```

---

## AD) Send Message API — Response Schema & Pacing Status

### Successful Send Response
```json
{
  "messaging_product": "whatsapp",
  "contacts": [
    {
      "input": "<WHATSAPP_USER_PHONE_NUMBER>",
      "wa_id": "<WHATSAPP_USER_ID>"
    }
  ],
  "messages": [
    {
      "id": "wamid.HBgL...",
      "message_status": "accepted"
    }
  ]
}
```

**`message_status` values** in send response:
- `"accepted"` — Message accepted by Meta's system (delivery not yet confirmed).
- `"held_for_quality_assessment"` — Message is being held (quality/pacing check in progress).

**Important**: A successful send response (HTTP 200) only confirms acceptance by Meta's API. It does NOT confirm delivery. Track actual delivery via status webhooks (`sent` → `delivered` → `read`).

### Media Link Caching
If you use `"link": "https://..."` instead of `"id"` for media in messages:
- Meta caches the asset for **10 minutes**.
- Same URL in multiple requests within 10 minutes → Meta uses the cached asset.
- To force a fresh fetch: append a random query string to the URL (`?v=<timestamp>`).

### Phone Number Format Rules
WhatsApp accepts phone numbers in multiple formats. The API normalizes them:
- `+91 98765 43210` → normalized to `919876543210`
- `0091-98765-43210` → same normalization
- Always include country code. No leading 0 after country code.
- Brazil and Mexico: extra prefix digits may be modified by Cloud API. This is expected behavior.

---

## AE) Pricing Country Rate Reference

**Effective July 1, 2025 (Per-Message Pricing)**. Rates vary by recipient country and message category. Charges are based on the **recipient's country code**, not where your business is located.

### High-Traffic Market Sample Rates (USD per message, approximate)

| Country | Marketing | Utility | Authentication |
|---|---|---|---|
| India | $0.0107 | $0.0040 | $0.0040 |
| Brazil | $0.0625 | $0.0125 | $0.0125 |
| United Kingdom | $0.0529 | $0.0180 | $0.0180 |
| Germany | $0.1365 | $0.0456 | $0.0456 |
| USA | $0.0250 | $0.0050 | $0.0050 |
| Indonesia | $0.0360 | $0.0085 | $0.0085 |
| Mexico | $0.0450 | $0.0110 | $0.0110 |
| Service (all countries) | Free | Free | Free |

> **Always check the live Meta rate card**: `https://business.whatsapp.com/products/platform-pricing`
> Rates change periodically. Hardcoding rates without a refresh mechanism is a SaaS anti-pattern.

### Billing Cycle & Payment
- Meta bills on a monthly basis.
- Local currency billing available in 16+ markets from 2026 (starting Mexico January 2026).
- WABA currency and timezone **cannot be changed** once a credit line is attached.
- Ensure each tenant WABA has a valid payment method. Failure to pay → `131042` errors.

---

## AF) On-Premises API Sunset (October 2025)

Meta officially **deprecated and shut down the On-Premises WhatsApp Business API** in October 2025.

**Cloud API is now the ONLY supported architecture.**

**Impact on The Platform**:
- Any tenant still on On-Premises must have migrated to Cloud API by now.
- Remove all On-Premises-specific code paths, references, and docs from The Platform codebase.
- `platform_type` field on phone numbers should now always return `CLOUD_API`.

---

## AG) Operational Runbook Snippets

### Webhook Processor Architecture (Recommended for The Platform)
```
Meta → HTTPS POST → Your Load Balancer (TLS termination)
         → Webhook Ingestion Service (Go/Rust)
              1. Validate X-Hub-Signature-256 (HMAC-SHA256 on raw body, constant-time compare)
              2. Return HTTP 200 immediately (< 5 sec)
              3. Publish raw payload to Redis Streams queue (stream: "wa:webhooks")
         → Webhook Processor Workers (consume Redis Streams)
              1. Deserialize payload
              2. Resolve WABA_ID + PHONE_NUMBER_ID → tenant context
              3. De-dupe check: Redis SET NX with key (PHONE_NUMBER_ID:WA_MESSAGE_ID:type) TTL 48h
              4. Route to handler: message | status | template_status | quality | call | account | user_preferences
              5. Persist to DB (webhook_events table)
              6. Trigger downstream: inbox update / notification / billing event / opt-out update
```

### Common Production Gotchas

| Gotcha | Mitigation |
|---|---|
| Same message triggers both `delivered` AND `failed` | Valid — user on multiple devices. Delivered wins. Mark as delivered. |
| Status webhooks arrive out of order (read before delivered) | Use `timestamp` field, not arrival order. Treat `read` as implying `delivered`. |
| Media URL expired by time you try to download | Immediately enqueue media download on webhook receipt. Re-fetch URL if needed. |
| Webhook batch has 100+ messages | Process all `entry[].changes[]` in a loop. Never assume one event per POST. |
| Flow `nfm_reply` has no `flow_token` in top-level | `flow_token` is in `interactive.nfm_reply` context. Key off `flow_token` from your send-side record. |
| `wa_id` ≠ phone number after 2026 Usernames rollout | Store `wa_id` as opaque string. Support both formats. |
| Template send fails with `132001` (template not found) | Sync approved templates via API on startup and on `message_template_status_update` webhook. |
| Payment gateway webhook arrives before WhatsApp status | Always reconcile via Payment Lookup API. Gateway webhook is informational only. |
| `131060` error (unsupported message type) on first contact | Usually transient for Coexistence. Wait 5s and retry. |
| Template category re-classified mid-campaign | `template_category_update` webhook. Pause campaign, re-price, alert tenant. |
| User opted out via `user_preferences` webhook | Suppress all future marketing sends to this user. Log the opt-out event. |
| TTL expired with no delivered webhook | Message was silently dropped. Mark as undelivered in your system. No retry. |
| Carousel template card button click | Arrives as `interactive.button_reply` with the `payload` set during template creation. |

### Template Sync Strategy
On app startup and every 15 minutes (or on `message_template_status_update` / `message_template_quality_update` / `template_category_update` webhooks):
```
GET /{Version}/{WABA_ID}/message_templates
  ?fields=id,name,status,quality_score,language,category,components,ttl
  &limit=100
```
Page through results (check `paging.cursors.after`). Store in your DB. Serve from DB for UI, not live API.

### Graph API Changelog
Always monitor for breaking changes: `https://developers.facebook.com/docs/graph-api/changelog`
WhatsApp-specific changelog: `https://developers.facebook.com/docs/whatsapp/cloud-api/support/changelog`

---

## AH) Marketing Messages (MM) API — Complete Reference

### Overview
Marketing Messages API for WhatsApp (formerly MM Lite, now generally available as of April 2025) is Meta's optimized delivery engine for outbound marketing templates. It runs parallel to Cloud API and uses Meta's AI-based delivery optimization.

Official docs: `https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview/`

### Key Benefits vs. Cloud API for Marketing
- AI-optimized delivery: messages reach users who are more likely to engage.
- Up to ~9–30% higher delivery rates in high-volume markets (tested in India with 12M messages, Jan 2025).
- Advanced performance metrics: benchmarks vs. similar businesses, tailored recommendations.
- GIF support (richer media format).
- Time-to-live (TTL) customization for marketing templates (not available in standard Cloud API).
- Automatic creative optimizations (image animation, filtering) — in testing.
- `marketing_lite` pricing category in status webhooks (same pricing as `marketing`).

### MM API vs. Cloud API — When to Use Which

| Use Case | Recommended API |
|---|---|
| Marketing broadcasts / campaigns | MM API |
| Utility messages (order updates, shipping) | Cloud API |
| Authentication (OTP) | Cloud API |
| Service messages (CSW replies) | Cloud API |
| Inbound message handling | Cloud API (webhooks) |
| User-triggered flows | Cloud API |

### MM API Endpoint
```
POST /{Version}/{PHONE_NUMBER_ID}/marketing_messages
Authorization: Bearer <ACCESS_TOKEN>
Content-Type: application/json
```

### MM API Send Payload
The schema is nearly identical to the Cloud API template message, with a few additions:
```json
{
  "messaging_product": "whatsapp",
  "to": "<USER_WA_ID>",
  "type": "template",
  "template": {
    "name": "summer_promo",
    "language": { "code": "en_US" },
    "components": [
      {
        "type": "header",
        "parameters": [
          { "type": "image", "image": { "id": "<MEDIA_ID>" } }
        ]
      },
      {
        "type": "body",
        "parameters": [
          { "type": "text", "text": "John" },
          { "type": "text", "text": "20%" }
        ]
      },
      {
        "type": "button",
        "sub_type": "url",
        "index": "0",
        "parameters": [
          { "type": "text", "text": "summer-sale" }
        ]
      }
    ],
    "ttl": {
      "seconds": 86400
    }
  },
  "tracking_data": {
    "pixel_id": "<PIXEL_ID>"
  }
}
```

### MM API Constraints
- Only supports **approved marketing templates**.
- Does NOT support interactive messages (buttons, lists, flows) as standalone messages — only within templates.
- Does NOT support service/utility/authentication templates (use Cloud API for those).
- Does NOT support session-based (free-form) messages.
- US phone number recipients: marketing delivery paused as of April 1, 2025.
- Uses the same WABA, phone number, and templates as Cloud API — no migration required.

### Onboarding to MM API
For The Platform as a Tech Provider, onboard client WABAs to MM API:
```
POST /{Version}/{BUSINESS_ID}/whatsapp_business_partner_onboarding_to_mm_lite_api
Body: {
  "waba_id": "<CLIENT_WABA_ID>"
}
```
Reference: `https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/whatsapp-business-partner-onboarding-to-mm-lite-api`

### MM API Status Webhook Pricing Category
Messages sent via MM API appear with `category: "marketing_lite"` in the status webhook pricing node:
```json
"pricing": {
  "billable": true,
  "pricing_model": "PMP",
  "category": "marketing_lite"
}
```
`marketing_lite` is billed at the same rate as `marketing`. Track separately in your analytics for MM vs. standard campaign comparison.

### MM API Metrics & Benchmarking
View campaign performance including benchmarks via:
```
GET /{Version}/{BusinessID}/template_analytics
  ?metric_types=SENT,DELIVERED,READ,CLICKED,BENCHMARK_READ,BENCHMARK_CLICK
  &template_ids=<ID1>,<ID2>
```
`BENCHMARK_READ` and `BENCHMARK_CLICK` are exclusive to MM API — they compare your template's performance against similar businesses in your region.

---

## AI) Conversational Components API — Full Reference

Conversational Components are in-chat features that make it easier for users to start conversations with your business phone number.

### Three Components
1. **Welcome Messages**: Fires a `request_welcome` webhook when a user opens a chat for the first time. You respond with a greeting.
2. **Ice Breakers (Prompts)**: Up to 4 tappable suggestion chips shown at the start of a conversation. User tapping one sends it as a regular text message.
3. **Commands**: Up to 30 slash-commands the user can discover by typing `/` in the chat.

### Configure Conversational Components (API)
```
POST /{Version}/{WABA_ID}/conversational_automation
Authorization: Bearer <ACCESS_TOKEN>
Content-Type: application/json
```

```json
{
  "enable_welcome_message": true,
  "prompts": [
    "Track my order",
    "View my account",
    "Talk to support",
    "Browse catalog"
  ],
  "commands": [
    {
      "command_name": "order",
      "command_description": "Track or manage your orders"
    },
    {
      "command_name": "help",
      "command_description": "Get help from our support team"
    }
  ]
}
```

**Note**: This applies to a specific phone number. Use the phone number's WABA token and specify the WABA the number belongs to. Alternatively, configure directly in WhatsApp Manager UI: Account Tools → Phone Numbers → Settings gear → Automations.

### Read Current Configuration
```
GET /{Version}/{WABA_ID}/conversational_automation
Returns: {
  "enable_welcome_message": true,
  "prompts": ["Track my order", ...],
  "commands": [{ "command_name": "order", ... }]
}
```

### Constraints

| Component | Limit | Format |
|---|---|---|
| Ice Breakers (prompts) | Max 4 | Max 80 chars each. No emojis. |
| Commands | Max 30 | Max 32 chars for name (no slash needed in payload — displayed with slash). Max 256 chars for description. No emojis. |
| Welcome message | 1 per number | Fires once per user per thread. |

### Welcome Message Webhook
When `enable_welcome_message: true` and a user opens a chat for the first time:
```json
{
  "from": "<USER_WA_ID>",
  "id": "wamid.HBgL...",
  "timestamp": "1669876545",
  "type": "request_welcome"
}
```
- This opens a CSW. Respond with any free-form or template message.
- If the user already had a chat thread, delete and restart the thread to re-trigger.
- `request_welcome` fires **once per user per thread lifetime**.

### Ice Breaker Behavior
- Shown when user opens chat for the first time.
- User tapping an ice breaker sends it as a plain text message.
- Dismissed if the user starts typing, or if they arrive via a `wa.me` link with pre-filled text.
- Handle as a regular inbound `text` webhook. Match body text to your ice breaker strings for routing.

### Commands Behavior
- Shown when user types `/` in chat.
- User selecting a command sends it as plain text: `/order`, `/help`, etc.
- Handle as a regular inbound `text` webhook. Route on the command string.

### SaaS Action: The Platform
- Expose ice breakers and commands configuration in the tenant's phone number settings UI.
- Map ice breaker responses and command messages to The Platform automation flows.
- Handle `request_welcome` type in your webhook router as a distinct event (trigger greeting automation).

---

## AJ) Business Encryption API

For businesses operating in regulated industries or requiring end-to-end encryption for message metadata, WhatsApp supports business-side encryption key management.

```
POST /{Version}/{PHONE_NUMBER_ID}/business_encryption
Body: {
  "business_public_key": "<YOUR_PUBLIC_KEY_PEM>"
}
```

```
GET /{Version}/{PHONE_NUMBER_ID}/business_encryption
Returns: {
  "business_public_key": "<CURRENT_PUBLIC_KEY_PEM>",
  "updated_at": 1669876543
}
```

This is used for: encrypting flow data exchange payloads, securing sensitive parameters in webhook payloads (e.g. payment data). Refer to official docs for complete key format and rotation requirements.

---

## AK) Partner Solutions — Deactivation Lifecycle

As a Tech Provider, you may need to manage partner deactivation requests when a customer wants to move to another provider.

### Send Deactivation Request (Customer to Partner)
```
POST /{Version}/{SOLUTION_ID}/send_deactivation_request
```

### Accept / Reject Deactivation
```
POST /{Version}/{SOLUTION_ID}/accept_deactivation_request
POST /{Version}/{SOLUTION_ID}/reject_deactivation_request
```

**`partner_solutions` webhook**: Fires when a deactivation request is created or its status changes:
```json
{
  "field": "partner_solutions",
  "value": {
    "event": "DEACTIVATION_REQUESTED",
    "solution_id": "<SOLUTION_ID>",
    "waba_id": "<WABA_ID>"
  }
}
```

**SaaS Rule**: Subscribe to `partner_solutions` webhook. Alert ops and account managers on any deactivation request. Do not silently drop. Log and track SLA for response.

---

## AL) Graph API Version Compatibility Matrix

| Feature | Minimum API Version | Notes |
|---|---|---|
| All basic messaging | v16.0+ | |
| Calling (USER/BUSINESS direction values) | v21.0+ | Mandatory after Jan 2, 2025 |
| MM API | v19.0+ | |
| Template TTL | v18.0+ | |
| Address messages | v17.0+ | India only |
| Location request messages | v16.0+ | |
| Conversational Components API | v17.0+ | |
| BSUID routing | v20.0+ | |
| Template carousel | v17.0+ | |
| Interactive media carousel | v17.0+ | |
| Groups management full API | v17.0+ | OBA required |
| Business Encryption API | v17.0+ | |
| Pre-Verified Phone Numbers | v19.0+ | |
| WABA Activities API | v18.0+ | |
| Schedules API | v19.0+ | |
| Coupon code templates | v16.0+ | |
| Limited-time offer templates | v16.0+ | |

**Recommended**: Pin to **v21.0** or later for all new integrations. Test thoroughly before version upgrades.

---

## AM) Reference Implementation: [Whatomate](https://github.com/shridarpatil/whatomate)

**Whatomate** serves as the primary open-source reference for the platform's WhatsApp integration logic. It implements the "Sovereign Messaging" model, focusing on:
- **Clean Event Processing**: Robust webhook handling and de-duplication.
- **Template Management**: Automated synchronization and lifecycle tracking.
- **Sovereign Control**: Self-hosted infrastructure ensuring 100% data privacy and compliance with the **Sovereign Master Taste** doctrine.

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard — BlackLoverTech*
