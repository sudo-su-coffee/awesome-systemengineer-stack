# Search & Discovery Architecture

If users cannot find the product, they cannot buy it. Standard SQL `LIKE '%coconut%'` queries are extremely slow on large databases and completely fail if the user makes a typo (e.g., typing "coconot").

## 1. The Meilisearch Engine
We will integrate an open-source, ultra-fast search engine like **Meilisearch** or **Typesense** alongside our PostgreSQL database.
- **Why?**: It is typo-tolerant, ranks results by relevance, and responds in under 10 milliseconds.

## 2. The Sync Pipeline
PostgreSQL remains our absolute source of truth. Meilisearch is just for speed.
1. When an Admin adds a new "Coconut Bowl" via the Go API, the Go backend inserts it into PostgreSQL.
2. Immediately after, Go sends a tiny JSON payload to the Meilisearch server to index the new item.
3. If an item goes out of stock, Go updates Postgres and tells Meilisearch to hide it.

## 3. The Frontend Experience
When a user opens the Flutter app and taps the search bar:
- As they type `C - o - k - o`, the Flutter app sends requests to the Go search API.
- The Go API queries Meilisearch.
- Even though they spelled it wrong, Meilisearch instantly returns "Coconut Bowls" in 5 milliseconds.
- The user sees live auto-complete results as they type, just like on Amazon!
