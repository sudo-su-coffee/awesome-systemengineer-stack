# 🤖 Workflow Automation Engineering Stack

This document defines the architecture for intelligent message routing, chatbot orchestration, and low-code integrations within the Whatomate ecosystem.

## 1. Chatbot Finite State Machine (FSM)

All conversational logic is modeled as a deterministic Finite State Machine (FSM) to ensure predictable user journeys.

- **Storage:** State persists in **HA Redis (T5)** with a TTL of 24h (sliding window).
- **Core App (T3):** Acts as the State Transition Engine.
- **Components:**
  - **State:** Current node in the flow (e.g., `START`, `AWAITING_PAYMENT`, `RESOLVED`).
  - **Input:** Webhook event (text, button tap, flow response).
  - **Transition:** Logic evaluating Input → Next State + Action.
  - **Action:** Outbound API call (Send Message, Trigger Webhook, Update CRM).

## 2. Visual Flow Builder Logic

Whatomate provides a React-based visual builder that generates a JSON "Blueprint".

- **Blueprint Schema:**
  ```json
  {
    "nodes": [
      { "id": "n1", "type": "trigger", "data": { "keyword": "hello" } },
      { "id": "n2", "type": "action", "data": { "message": "Welcome!" } }
    ],
    "edges": [{ "source": "n1", "target": "n2" }]
  }
  ```
- **Execution:** The Go backend pre-compiles these Blueprints into a directed acyclic graph (DAG) for O(1) traversal during execution.

## 3. n8n Integration (The Glue)

For complex, third-party integrations (Google Sheets, Salesforce, Custom APIs), we leverage **self-hosted n8n**.

- **Pattern:** Whatomate App → Webhook Node (n8n) → Business Logic → Response (Whatomate API).
- **Security:** n8n calls are authenticated via scoped API tokens (Tier 13 Sandbox rules apply).
- **Role:** n8n is used for "Elastic Logic" that doesn't belong in the high-performance Go core.

## 4. Trigger / Action Patterns

| Pattern | Source | Target | Delivery |
|---|---|---|---|
| **Inbound Message** | WhatsApp Webhook | Chatbot FSM | NATS (T11) → Task Worker (T4) |
| **Schedule** | Lago / Campaign | Bulk Send Worker | NATS (T11) Group Queue |
| **System Event** | Core App | Webhook / n8n | Outbound HTTP Hook |

## 5. NATS Event Routing (T11)

**NATS.io** is the central nervous system for all automation events.

- **Subjects:**
  - `wa.inbound.{tenant_id}`: All incoming WhatsApp messages.
  - `automation.flow.{id}`: Triggered flow executions.
  - `crm.update`: Contact property changes.
- **QoS:** We use NATS JetStream for **at-least-once** delivery of critical events (Billing, Campaigns).
- **Performance:** Sub-millisecond latency for event propagation across the App Mesh.

## 6. Implementation Example (Transition Engine)

```go
type StateMachine struct {
    TenantID string
    Cursor   string // Current node ID
}

func (s *StateMachine) HandleInput(ctx context.Context, input string) (*Transition, error) {
    node := s.Blueprint.GetNode(s.Cursor)
    for _, edge := range node.Edges {
        if edge.Match(input) {
            return &Transition{NextID: edge.Target, Effects: edge.Actions}, nil
        }
    }
    return nil, ERR_AUTOMATION_NO_MATCH
}
```

## 7. Advanced Workflow Engines & Scheduling (Sovereign)

For high-performance, developer-first automation and human-centric scheduling, the platform integrates the following sovereign standards:

- **Workflow Engine:** **[Windmill](https://github.com/windmill-labs/windmill)** — An open-source developer platform to power entire infrastructure and turn scripts into webhooks, workflows, and UIs. Replaces/complements Retool and Temporal with a faster, script-native execution model.
- **Scheduling Infrastructure:** **[Cal.com](https://github.com/calcom/cal.com)** — Scheduling infrastructure for absolutely everyone. Self-hosted scheduling that integrates directly with the WhatsApp inbox and CRM for automated appointment booking.

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard — BlackLoverTech*
