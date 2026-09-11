# 📽️ CONTENT INTELLIGENCE API (GO + MINIO)

This specification defines the architecture for "The Engine"—a high-performance Content Intelligence API designed to serve the Scriptpapi video library and associated high-fidelity assets (Lottie, Vector).

---

## 🏗️ SYSTEM ARCHITECTURE OVERVIEW

### 1. The Core Stack
- **Language:** Go (Golang) — utilizing `net/http` or `Gin` for high-throughput REST.
- **Data Layer:**
    - **PostgreSQL:** For structured metadata (Video titles, tags, skill graph links).
    - **Redis:** For edge-caching frequently accessed SDUI schemas and asset manifests.
- **Storage Layer:**
    - **MinIO:** Self-hosted object storage for raw video files (MP4/HLS) and assets (JSON/VEC).
- **Communication:**
    - **REST API:** For public content discovery.
    - **gRPC:** For internal service-to-service communication (e.g., between the Content Engine and the Notification Engine).

---

## ⛓️ DATA MODEL: THE CONTENT GRAPH

Instead of a flat list, content is stored as a **Directed Acyclic Graph (DAG)** to represent learning paths.

### A. Video Entry Schema
```go
type VideoContent struct {
    ID          string   `json:"id"`
    Title       string   `json:"title"`
    Description string   `json:"description"`
    Category    string   `json:"category"`
    Prereqs     []string `json:"prerequisites"` // Skill Graph IDs
    Assets      Assets   `json:"assets"`
    Source      Source   `json:"source"`
}

type Assets struct {
    LottieID string `json:"lottie_id"` // ID in MinIO
    VectorID string `json:"vector_id"` // ID in MinIO
    PosterURL string `json:"poster_url"`
}

type Source struct {
    Provider string `json:"provider"` // "YouTube" or "MinIO"
    RemoteID string `json:"remote_id"`
    HLS_URL  string `json:"hls_url"`    // For self-hosted playback
}
```

---

## 🪣 MINIO INTEGRATION & HYDRATION

The API handles **Asset Hydration** by providing Signed URLs or Serialized Paths.

### 1. Storage Buckets
- `v-raw/`: Original video uploads.
- `v-hls/`: Transcoded playback fragments.
- `v-assets/`: Lottie animations and Vector (.vec) files.

### 2. Signing Logic
When a client requests a video, the Go server:
1. Validates the user's `JWT` (OIDC).
2. Fetches metadata from Postgres.
3. Generates **Presigned URLs** for the MinIO assets (TTL: 1 hour).
4. Packages a **SDUI Schema** for the mobile cards.

---

## 📱 SDUI & DYNAMISM

The API doesn't just return data; it returns the **UI Layout instructions**.

### Sample SDUI Response Snippet (REST)
```json
{
  "type": "video_card_v1",
  "data": {
    "title": "Apple Pay Decryption Tutorial",
    "animation_url": "https://oss.sovereign.local/v-assets/pay_anim.json?sig=...",
    "prereq_status": "locked",
    "on_tap": {
      "action": "navigate_to_video",
      "video_id": "vid_0x88"
    }
  }
}
```

---

## 🚀 SCALABILITY & PERFORMANCE
- **Parallel Transcoding:** Utilizing Go workers to trigger `ffmpeg` jobs upon MinIO upload (via S3 Webhooks).
- **Vector Serialization:** To achieve "Zero-Latency Icons," the API can inject small SVG paths directly into the JSON payload (Base64 or raw paths) to avoid extra network requests.
- **ClickHouse Analytics:** View events are piped through a buffer into ClickHouse for high-speed engagement reporting and skill-gap analysis.
