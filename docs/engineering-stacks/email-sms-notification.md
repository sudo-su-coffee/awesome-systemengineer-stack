# 📧 Email, SMS & Notification Engineering Stack

This document defines the architecture for high-performance, transactional, and marketing message delivery outside the WhatsApp platform.

## 1. Hedwig Delivery Engine (Rust)

For core delivery tasks that require deterministic performance and safety, we use **Hedwig**—our specialized Rust-based notification service.

- **Why Rust?** Zero-cost abstractions and memory safety for high-concurrency binary processing (Template rendering + SMTP/API calling).
- **Concurrency:** Uses `tokio` for non-blocking I/O, allowing a single node to handle >5,000 TPS.
- **Integration:** Communicates with the Go API Mesh via **gRPC / ProtoBuf**, minimizing serialization overhead.

## 2. Transactional Email & SMS Routing

We adopt a provider-agnostic routing layer to prevent vendor lock-in and ensure maximum deliverability.

| Channel | Providers | Mechanism |
|---|---|---|
| **Email** | Postmark / AWS SES / Resend | SMTP or JSON API |
| **SMS** | Twilio / MessageBird / Plivo | REST API |
| **Push** | FCM (Android) / APNS (iOS) | HTTP/2 (gRPC) |

- **Routing Rules:** Failover logic (e.g., if Postmark returns 5xx, retry with SES).
- **Templates:** **Typst** (Rust) for PDF attachments and **Handlebars** for HTML emails.

## 3. Push Notification Stack (T10)

High-priority mobile push notifications are handled via Firebase Cloud Messaging (FCM) and Apple Push Notification Service (APNS).

- **Payload Standard:** All push notifications follow a "Data-Only" payload to allow the Flutter app to handle display logic (using `flutter_local_notifications`).
- **Silent Pushes:** Used for background data syncing (e.g., refreshing the conversation list).
- **Grouping:** All notifications are tagged with a `thread_id` to ensure proper OS-level thread grouping.

## 4. Notification Service Patterns (Zulip/Z-Standard)

Following the **Zulip** engineering philosophy, we optimize for "User Disengagement":

- **Aggregation:** If 5 messages arrive in 10 seconds, we send a single "You have 5 new messages" push instead of 5 individual alerts.
- **Downtime Buffering:** If the user is offline (detected via WebSocket pulse), notifications are queued in **HA Redis (T5)** and flushed on reconnect.
- **Mute / Quiet Hours:** Managed via user-defined policies checked in the delivery pipeline.

## 5. Event Flow Architecture

1. **Trigger:** `internal.event.notify` published to **NATS (T11)**.
2. **Aggregator:** Task Worker (T4) consumes, batches, and checks mute policies.
3. **Dispatcher:** Hedwig (Rust) receives gRPC call and selects the cheapest/most reliable route.
4. **Outcome:** Results (Delivered, Bounced, Failed) logged to **ClickHouse (T7)** for analytics.

## 6. Implementation Example (Rust Dispatcher)

```rust
pub async fn send_email(req: EmailRequest) -> Result<Response, Error> {
    let provider = match req.priority {
        Priority::High => &POSTMARK_CLIENT,
        _ => &SES_CLIENT,
    };
    
    provider.dispatch(req).await
}
```

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard*
