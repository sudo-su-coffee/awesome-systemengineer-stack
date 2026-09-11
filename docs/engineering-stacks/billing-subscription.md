# 💳 Billing & Subscription Engineering Stack

This document defines the architecture for usage-based metering, subscription lifecycle, and automated dunning for the Whatomate SaaS platform.

## 1. Governance with Lago

We standardize on **Lago** (Economic Engine) for all metering and billing logic.

- **Mechanism:** The Core App (T3) or Worker (T4) emits "Events" to Lago for every billable action (e.g., Message Sent, AI Tokens used, Active Contact count).
- **WAL-Backed:** All events are first committed to a Write-Ahead Log (WAL) to ensure billing accuracy even during network partitions.
- **Aggregation:** Lago handles the hourly/daily aggregation of these events based on the tenant's plan.

## 2. Plan Tiers & Entitlements

We manage entitlements via Lago's plan engine, which are enforced at the API Gateway (T1).

| Tier | Messaging Quota | Features |
|---|---|---|
| **Free** | 1,000 / mo | Community Support, 1 Agent |
| **Pro** | 50,000 / mo | 24/7 SLA, 5 Agents, API Access |
| **Enterprise** | Unlimited | Dedicated Qdrant, High-Performance Hedwig |

- **Enforcement:** Before sensitive operations (e.g., starting a Campaign), the app checks the tenant's current usage against their Lago allowance.

## 3. WhatsApp-Native Dunning Logic

Following the "Zero JS Bridge" and "Customer Service Window" rules, we handle payment failures directly on WhatsApp.

- **Trigger:** Lago fires a `payment.failed` webhook.
- **Outcome:** The App sends a **WhatsApp Template** to the account owner with a **Pay Now** button.
- **Grace Period (FSM):**
  - **Day 1-3:** Daily reminders on WhatsApp.
  - **Day 7:** Downgrade to "Free" tier automatically.
  - **Day 30:** Account suspension and WABA disconnection.

## 4. Merchant of Record (MoR) Integration

We use **Hyperswitch (Juspay)** to route payments across multiple providers (Stripe, Razorpay, Paddle).

- **Role:** Juspay acts as the Vault for credit cards, ensuring we never handle raw PCI data.
- **Failover:** If Razorpay (India) is down, Juspay automatically routes the payment retry to Stripe.
- **Ledger:** All transactions are logged with UUIDv7 IDs in the Primary DB (T6) before being synced to Lago for reconciliation.

## 5. Implementation Example (Metering Event)

```go
func EmitBillingEvent(tenantID, code string, properties map[string]any) {
    event := &lago.Event{
        TransactionID: uuid.NewV7().String(),
        ExternalID:    tenantID,
        Code:          code,
        Properties:    properties,
    }
    
    go lagoClient.CreateEvent(ctx, event)
}
```

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard*
