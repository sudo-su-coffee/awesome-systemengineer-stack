# Real-Time Architecture: Redis + FCM Synergy

In modern backend architecture, you do not want your main Go API to be blocked or slowed down by talking to external services like Google/Firebase. To achieve maximum throughput (thousands of requests per second), we use **Redis** and **FCM (Firebase Cloud Messaging)** together in a highly optimized pipeline.

## 1. Why combine Redis and FCM?
- **FCM** is the vehicle. It delivers messages (Silent Data or Push Notifications) to iOS and Android phones instantly without draining the battery.
- **Redis** is the engine. It acts as an ultra-fast message broker, queue, and timer that tells the Go backend *when* and *who* to send the FCM messages to.

## 2. Use Case 1: The Asynchronous Notification Queue
If an admin clicks "Send Promotional Blast to 100,000 Users", we cannot have the Go API loop through 100,000 rows in PostgreSQL and call the Firebase API 100,000 times synchronously. The HTTP request would time out and crash.

**The Solution:**
1. The Admin clicks send. The Go API instantly pushes 100,000 `user_id` messages into a **Redis List/Queue** (e.g., `RPUSH promo_queue`).
2. The Go API immediately returns a `200 OK` to the Admin panel in 5 milliseconds.
3. In the background, several Go worker routines (`Goroutines`) read from the Redis queue (`BLPOP promo_queue`), fetch the user's `FCMToken` from Postgres, and send the FCM payload asynchronously. 

## 3. Use Case 2: Time-Delayed Events (Redis Keyspace Notifications)
We use this specifically for **Abandoned Cart Recovery**. We need to send an FCM push notification exactly 30 minutes after a cart is abandoned. 

**The Solution:**
1. When a user adds to cart, Go creates a Redis Key with a 30-minute Time-To-Live (TTL): `SETEX cart_timeout:user_99 1800 "pending"`.
2. Go subscribes to Redis' internal Expiry Channel (`__keyevent@0__:expired`).
3. Exactly 30 minutes later, Redis deletes the key and fires an event.
4. The Go worker hears the event, looks up `user_99`'s FCM token, and fires the Firebase Cloud Message.

## 4. Use Case 3: Live Order Tracking (Pub/Sub)
If a delivery driver updates their location or an admin marks an order as "Shipped", we need that update to hit the user's phone instantly.

**The Solution:**
1. The driver app calls the Go API: `PUT /api/v1/orders/123/status`.
2. The Go API updates PostgreSQL and instantly publishes the event to Redis: `PUBLISH order_updates '{"order_id": 123, "status": "shipped"}'`.
3. A dedicated Go FCM Worker is constantly listening to the `order_updates` Redis channel.
4. The moment it hears the publish, it grabs the payload and sends a **Silent FCM Data Message** directly to the user's phone.
5. The Flutter app wakes up, catches the silent payload, and updates the screen—giving the illusion of a live WebSocket connection without the battery drain!
