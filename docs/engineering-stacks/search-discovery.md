# 🔍 Search & Discovery Engineering Stack

This document defines the architecture for high-performance, ultra-low latency search and discovery for messages, contacts, and metadata.

## 1. Search Engine (T12)

We standardize on **Meilisearch** (primary) and **Typesense** (fallback for large-scale faceting) as our dedicated search engines.

| Engine | Role | Why |
|---|---|---|
| **Meilisearch** | Instant Global Search | Typo-tolerance, sub-50ms latency, developer-friendly JSON API |
| **Typesense** | Advanced Discovery | Superior performance for complex faceting and large datasets |

## 2. Multi-tenant Index Design

To prevent cross-tenant data leaks and ensure optimal performance, we enforce strict index isolation.

- **Index Naming:** `tenant_{tenant_id}_{collection_name}` (e.g., `tenant_99_messages`).
- **Scoping:** Every search query MUST include a mandatory `tenant_id` filter (though using per-tenant indexes is preferred for physical isolation).
- **Security:** Scoped API keys are used for the frontend search, limited to the specific tenant's indexes.

## 3. Data Sync & Indexing Lifecycle

We treat our Primary DB (PostgreSQL T6) as the source of truth and async-sync to Meilisearch.

1. **Trigger:** Core App (T3) or Worker (T4) updates a record.
2. **Event:** `search.index_update` published to **NATS (T11)**.
3. **Consumer:** Task Worker (T4) processes the event and pushes to the Meilisearch API.
4. **Consistency:** Periodic reconciliation jobs run every 24h to ensure the index matches the DB for any missed events.

## 4. Faceting & Filtering

We support rich discovery via facets (filter categories):

- **Contacts:** Status (Lead, VIP, Blocked), Tags, Assigned Agent, City.
- **Messages:** Direction (Inbound/Outbound), Type (Text, Image, PDF), Sent Date (Buckets).
- **Campaigns:** Outcome (Delivered, Clicked), Start Date.

## 5. Query Normalization

To handle various input styles and languages (especially in multi-lingual India/EU regions):

- **Tokenization:** Automatic handled by Meilisearch's analyzer.
- **Stopwords:** We maintain custom lists per locale to improve relevance.
- **Synonyms:** e.g., "hi", "hello", "hey" → normalized to a common intent for discovery.

## 6. Implementation Example (Search Query)

```go
func SearchMessages(tenantID, query string, filters []string) ([]Message, error) {
    indexName := fmt.Sprintf("tenant_%s_messages", tenantID)
    searchRequest := &meilisearch.SearchRequest{
        Filters: strings.Join(filters, " AND "),
        Limit:   20,
    }
    
    resp, err := client.Index(indexName).Search(query, searchRequest)
    // Map to Go struct...
}
```

---
*Last Updated: April 2026 | Universal SaaS Engineering Standard*
