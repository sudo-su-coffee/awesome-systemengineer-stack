# 👥 CRM & Contacts Engineering Stack

This document defines the architecture for managing customer identities, conversation lifecycles, and agent workflows within the Whatomate SaaS.

## 1. Contact Model (The Dual-Key)

To support the 2026 WhatsApp rollout of Usernames, our contact model adopts a dual-key primary identification system.

- **Primary Identity:** `phone_number` (Classic E.164 format).
- **Secondary Identity:** `BSUID` (Business-Specific User ID). 
- **Mapping:** Every `Contact` record links these two. If a user opts into a username-only interaction, the `BSUID` becomes the lead identifier for outbound routing.
- **Tenant Scope:** Contacts are private to a `tenant_id`. Cross-tenant contact sharing is physically blocked by PostgreSQL RLS.

## 2. Conversation Lifecycle (State Machine)

A "Conversation" is a metadata layer over a stream of messages between a Guest and a Tenant.

- **States:** `NEW` (Unassigned) → `ACTIVE` (Assigned) → `PENDING` (Waiting for user) → `RESOLVED` (Closed).
- **Session Rule:** A conversation session is considered "Expired" if no messages are exchanged for 24h (Matching WhatsApp's pricing window).
- **Re-opening:** A new inbound message in a `RESOLVED` state triggers a transition back to `NEW` or `ACTIVE` based on auto-assignment settings.

## 3. SLA Finite State Machine (FSM)

Service Level Agreements (SLAs) are enforced via an background FSM that monitors conversation timestamps.

- **T1 Responde Time:** Target < 5 minutes.
- **T2 Resolution Time:** Target < 4 hours.
- **Escalation:** If a state remains `NEW` for > 15 mins, an `ERR_SLA_BREACH` event is fired to **NATS (T11)**, triggering a manager notification.

## 4. Agent Assignment Logic

Assignment is handled by a configurable routing engine (`internal/assignment`).

- **Round Robin:** Distributes `NEW` conversations equally among online agents.
- **Sticky Assignment:** If a contact has an "Owner" attribute, they are always routed to that specific agent first.
- **Cascading:** If the Sticky Agent is offline, the conversation falls back to the Round Robin pool.

## 5. Tagging & Custom Fields

We support a "Schemaless-Lite" approach for contact metadata.

- **Tags:** Simple string array (e.g., `["VIP", "Refused_Offer", "Support_Pass"]`).
- **Custom Fields:** Key-Value pairs stored in a **JSONB** column in PostgreSQL.
- **Indexing:** Meilisearch (T12) indexes all tags and custom fields for instant discovery.

## 6. Implementation Example (Contact Schema)

```go
type Contact struct {
    ID           string         `gorm:"primaryKey;type:uuid"`
    TenantID     string         `gorm:"index;not null"`
    Phone        string         `gorm:"index"`
    BSUID        string         `gorm:"index"`
    Name         string
    Tags         []string       `gorm:"type:text[]"`
    Properties   json.RawMessage `gorm:"type:jsonb"`
    Status       string         `gorm:"default:'lead'"`
}
```

## 7. Sovereign CRM & Messaging Alternatives

To provide flexibility for diverse customer engagement scenarios, the following high-agency platforms are integrated / supported:

- **Core CRM:** **[Twenty](https://github.com/twentyhq/twenty)** — A modern, open-source alternative to Salesforce. Used for deep relationship management and sales pipelines alongside the Whatomate messaging core.
- **Live Chat:** **[Papercups](https://github.com/papercups-io/papercups)** — Open-source live customer chat. Provides a web-based chat widget that integrates directly into the platform's conversation lifecycle.
- **Team Messaging:** **[Raven](https://github.com/The-Commit-Company/raven)** — A simple, open-source team messaging platform for internal coordination and agent collaboration.

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard — BlackLoverTech*
