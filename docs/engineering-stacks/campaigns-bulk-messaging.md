# 📣 Campaigns & Bulk Messaging Engineering Stack

This document defines the architecture for sending millions of WhatsApp messages while remaining compliant with Meta's quality standards and maintaining high system throughput.

## 1. Bulk Worker Architecture (T4)

We use a horizontally scalable worker pool (`task-worker` Tier 4) to handle large-scale message dispatch.

- **Queue Service:** **NATS JetStream (T11)** acts as the durable persistent queue.
- **Worker Logic:** Workers pull job slices and execute individual `/messages` API calls.
- **Throttling:** Each worker respects a per-tenant rate-limit stored in **HA Redis (T5)** to prevent Meta account suspensions.

## 2. Template Pacing & Quality Guard

Meta automatically scales messaging tiers based on template quality. We implement defensive pacing:

- **Warm-up Mode:** For new accounts, the campaign engine throttles initial sends (e.g., 50 messages/min) regardless of the Meta tier.
- **Sentiment Monitoring:** We analyze status webhooks (Read vs. Failed vs. Blocked). If the `blocked` rate exceeds 5%, the campaign is auto-paused, and a `ERR_CAMPAIGN_QUALITY_ISSUE` trigger is fired.
- **Rate-Limit Compliance:** All workers use a Token Bucket algorithm (Go `x/time/rate`) to strictly adhere to the `v21.0` Cloud API throughput limits.

## 3. Opt-out & GDPR Suppression List

Compliance is non-negotiable. We maintain a centralized suppression list.

- **Opt-out Detection:** Chatbot FSM (Tier 3) monitors for keywords like "STOP", "UNSUBSCRIBE", or "REMOVE".
- **Suppression:** Before any bulk send, the worker checks the suppression list (indexed in Redis T5 for O(1) lookups).
- **GDPR Audit:** Every bulk send event is logged to **ClickHouse (T7)** with the Opt-out status at the time of sending to prove compliance during audits.

## 4. Job Scheduling & Batching

- **Scheduling:** **Lago (Economic Engine)** or custom cron jobs trigger campaign starts.
- **Batching:** We batch sends locally in workers to minimize NATS roundtrips.
- **Idempotency:** Every message in a campaign is assigned a unique `bulk_job_id` + `contact_id` idempotency key. If a worker crashes, the re-queueing will not result in double-sends.

## 5. Implementation Example (Bulk Dispatcher)

```go
func DispatchCampaign(ctx context.Context, job CampaignJob) {
    limiter := rate.NewLimiter(rate.Limit(job.RatePerSecond), 1)
    
    for _, contact := range job.Contacts {
        if IsSuppressed(contact.ID) {
            continue
        }
        
        limiter.Wait(ctx)
        go sendMessage(contact, job.TemplateID)
    }
}
```

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard*
