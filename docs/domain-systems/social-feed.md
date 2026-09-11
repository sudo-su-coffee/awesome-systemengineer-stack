# Social Graph & News Feed Architecture

If you want to build a social platform like **Instagram, X (Twitter), or LinkedIn**, the challenge is massive Read-Throughput and complex relationships.

## 1. The Graph Database Problem
In standard apps, you use Relational DBs (Postgres). In social apps, you have to answer complex queries fast: *"Find all friends of John who liked a post by Sarah."*
- Doing this in Postgres requires 5 slow `JOIN` statements.
- Top social apps use **Graph Databases (like Neo4j or Amazon Neptune)**. Data is stored as Nodes (Users) and Edges (Follows, Likes). Graph queries answer these relationships in 2 milliseconds.

## 2. The Fan-Out Architecture (The Justin Bieber Problem)
When a normal user with 10 followers posts a photo, the Go backend simply inserts the post into the DB, and the 10 followers fetch it on their next load.
**What happens when someone with 100 Million followers posts a photo?**
If 100 Million users query the database at the same time, the server instantly crashes.
- **Fan-Out on Write**: When the celebrity posts, the Go backend doesn't wait for users to ask for it. It instantly pushes the post into a **Redis In-Memory Cache** for all 100 Million followers' timelines simultaneously.
- When followers open the app, they read directly from Redis (RAM) instead of hitting the Postgres hard drive.

## 3. Media Processing Pipelines
Users upload 4K videos from their iPhones. You cannot stream a 1GB video to someone on a 3G network.
- The Go backend uploads the raw video to an S3 Bucket and triggers an **AWS MediaConvert** worker.
- The worker transcodes the video into HLS (HTTP Live Streaming) format (1080p, 720p, 480p) and chunks it into 2-second segments. The Flutter app automatically switches to 480p if the user's internet slows down.
