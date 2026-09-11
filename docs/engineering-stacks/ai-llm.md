# 🧠 Universal SaaS: AI & LLM Engineering Stack

This document defines the **Intelligence Layer** of the platform—synthesizing **Industrial RAG patterns**, **Deterministic Prompt Orchestration**, and **Multi-tenant AI Safety**. It is the blueprint for a high-performance, cost-efficient LLM integration.

---

## 🏛️ 01. The RAG Maturity Model (Industrial Retrieval)

We move beyond "Naive RAG" toward a deterministic, multi-stage retrieval pipeline.

### 1. Hybrid Search (Vector + BM25)
To handle industrial terminology (e.g., specific error codes or hardware IDs), we combine:
- **Dense Vector Search**: Capturing semantic intent (Qdrant/Milvus).
- **Sparse Keyword Search (BM25)**: Capturing exact keyword matches for identifiers.
- **Reciprocal Rank Fusion (RRF)**: Merging the two results for optimal precision.

### 2. Cross-Encoder Re-ranking
After retrieval, we use a **Re-ranker** (e.g., Cohere/BGE) to re-evaluate the top 10 chunks against the query. This ensures only the most contextually relevant data enters the LLM's limited context window.

---

## ⚡ 02. Prompt Orchestration & Caching

LLM calls are expensive and latent. We minimize both through aggressive caching.

### 1. Prefix Caching (Provider Level)
We structure all prompts to maintain stable "Prefixes."
- **Pattern**: `[System Instructions] + [Tenant Context] + [User Query]`.
- **Optimization**: By keeping the System and Tenant context at the beginning, the LLM provider (Anthropic/OpenAI) can cache the KV-states, reducing Time-to-First-Token (TTFT) by up to 50%.

### 2. Semantic Caching (Redis)
- We use a **Vector Cache in Redis**.
- **Logic**: If a new query is semantically identical (> 0.98 similarity) to a previous cached query, we return the cached response instantly without hitting the LLM.

---

## 🛡️ 03. Multi-tenant AI Safety (Vector RLS)

Data leakage at the vector layer is a critical failure.

- **Metadata-Enforced Retrieval**: Every vector search query MUST include a filter on the `tenant_id` metadata field.
- **Partitioning**: For "High-Sovereignty" tenants, we utilize dedicated vector namespaces or collections to ensure physical isolation at the index level.

---

## 📈 04. AI Observability & "Judge" Framework

We don't guess if the AI is performing; we measure it.

1.  **LLM-as-a-Judge (Ragas)**:
    - We use a higher-tier LLM (e.g., GPT-4o/Claude 3.5) to periodically score production RAG outputs.
    - **Metrics**: Groundedness (Is the answer in the context?), Faithfulness, and Answer Relevance.
2.  **Traceability**:
    - Every AI response is linked to a **Trace ID** in ClickHouse that captures the exact chunks retrieved, the prompt version used, and the latency of each stage.

---

## 💰 05. The Economic Guardrails (LLM Rate-Limiting)

To protect the platform's billing margins:

- **Token Management**: Strict per-tenant token quotas enforced via **Redis Fixed-Window limits**.
- **Model Fallback**: If a high-tier model (Claude-3-Opus) hits a rate limit, the system gracefully falls back to a lower-cost model (Claude-3-Haiku) to maintain operational continuity.

---
*Last Updated: April 2026 | Following the Universal SaaS Engineering Standard.*
