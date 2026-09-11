# 💼 Economic & Business Engine
> **BlackLoverTech Industrial Standard** | April 2026
> [← Master Index](file:///d:/brain/source/Master_Source_Index.md) | Status: **Sovereign**

This document defines the **Commercial & Operational** layer—the engine that powers the ecosystem's growth. It codifies the financial integrity of the ledger, the sovereignty of contact management, and the industrial integration of the WhatsApp Business Platform.

---

## 🏛️ 1. Financial Execution & Routing (Hyperswitch)
We adopt **Juspay's deterministic state orchestration** to ensure absolute financial integrity and Zero-Drift ledgers.
- **Smart Routing**: **Hyperswitch** orchestrates across Razorpay (Primary India), Stripe (Primary US/EU), and Paytm (Fallback).
- **Multi-Provider Failover**: If a primary gateway returns 5xx or latency exceeds 2s, Hyperswitch automatically routes the retry to the next stable provider.
- **The Ledger of Record**: All transaction metadata is committed to **Tier 6 (Truth)** with UUIDv7 traceability before being asynchronously reconciled in **Tier 7 (Analysis)**.
- **The Vault**: Unified card tokenization via the Hyperswitch vault ensures maximum portability and zero PCI-compliance overhead for the core API.

---

## 💳 2. Billing, Metering & Onboarding

### Governance & Metering (Lago)
- **Consumption Pipeline**: Core App (T3) or Workers (T4) emit usage events (e.g., `msg_sent`, `ai_token_used`) to **Lago**.
- **Accuracy Protocol**: Events are first committed to the **Write-Ahead Log (WAL)** in ClickHouse to ensure billing integrity during network partitions.
- **WhatsApp Dunning**: Payment failures trigger automated **WhatsApp Templates** with one-click "Pay Now" buttons, utilizing the active 24h Customer Service Window.

### Multi-tenant Provisioning
- **Embedded Signup v4**: Programmatic onboarding of tenant WABAs via Meta's latest OAuth2 flow, ensuring zero-touch setup for merchants.
- **Asset Sovereignty**: Logos, Favicons, and App-Icons are served from the **Media Hub (T10)** and cached at the **Edge (T1)** for 100% white-labeled consistency.

---

## 👥 3. CRM & Contact Sovereignty

### Identity Management
- **Dual-Key Lifecycle**: Every contact record is keyed by both **Phone Number (E.164)** and the 2026 **BSUID** (Business-Scoped User ID). Outbound routing prioritizes BSUID for users opting into WhatsApp Usernames.
- **State Machine**: Deterministic transitions: `NEW` (Unassigned) → `ACTIVE` (Assigned) → `PENDING` (Waiting) → `RESOLVED` (Closed).
- **SLA Finite State Machine**: Background workers monitor conversation timestamps: T1 (First Response < 5m) and T2 (SLA Resolution < 4h). Breach events are fired to **NATS (T11)** for supervisor escalation.

---

## 📣 4. Campaigns & Multi-Channel Delivery

### Bulk Dispatch (Hedwig)
- **Hedwig Engine**: Specialized Rust-based delivery service (`tokio`).
- **WhatsApp Pacing**: Defensive throttling; auto-pause if "Blocked" rate > 5%.
- **Suppression**: Mandatory O(1) Redis check (T5) against opt-out lists.
- **Aggregation**: "User Disengagement" philosophy—aggregating alerts.

---

## 🤖 5. Workflow Automation & Extensibility

### Chatbot FSM
- **Execution**: Go-backend pre-compiles visual blueprints into DAGs.
- **State Persistence**: HA Redis (T5) with a 24h sliding window.
- **n8n Glue**: For complex 3rd-party logic (Salesforce/Sheets).

### Marketplace & SDK (The Wasm Standard)
- **Sandboxing**: 3rd-party plugins MUST be compiled to **WebAssembly (Wasm)** and executed via a high-performance runtime (Wasmtime) embedded in the Command Engine (T3).
- **Resource Jailing**: Every extension is isolated within strict CPU/Memory quotas (50MB RAM ceiling). Rogue processes are instantly killed by the platform watcher.
- **The Wasm ABI**: Plugins interact with the platform only through the **Whatomate Application Binary Interface (ABI)**, ensuring plugins never have direct network or filesystem access.

---

## 📱 6. WhatsApp Business Platform (Cloud API) Standards

### Reliability Contract
- **Webhook Timeout**: 5-second hard limit. Always respond 200 immediately.
- **Deduplication**: 48h Redis bloom filter (T5) for `(PHONE_NUMBER_ID, WA_MESSAGE_ID)`.
- **Media Strategy**: Mirror Meta media to internal S3 (T10) immediately.

### Pricing Protocol (July 2025 PMP Model)
Meta's moves away from Conversation-Based Pricing toward **Per-Message Pricing**.
- **Marketing**: Charged per delivered message. Includes a **72-hour Free Entry Point (FEP)** window for messages originating from Facebook/Instagram ads.
- **Utility/Auth**: Charged per delivered message, with volume-tier discounts for large-scale enterprise delivery.
- **Service Conversations**: 100% free with no monthly cap for any user-initiated interaction within the 24h Customer Service Window (CSW).

---
*Created by Antigravity | Commercial Excellence — BlackLoverTech*
> [↑ Return to Command Center](file:///d:/brain/source/Master_Source_Index.md)
