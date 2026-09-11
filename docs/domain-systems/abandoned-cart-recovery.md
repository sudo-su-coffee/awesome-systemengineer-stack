# Abandoned Cart Auto-Recovery: Feature Plan & Case Study

E-commerce data shows that roughly **70% of shopping carts are abandoned** before checkout. If you do not have a system to automatically follow up with these users, you are leaving massive amounts of money on the table.

An **Abandoned Cart Recovery System** runs in the background of our Go server. It detects when a user adds something to their cart but doesn't buy it, waits exactly 30 minutes, and automatically sends a push notification to their phone to bring them back.

---

## 📊 Case Study: The "FOMO" Push Notification

Companies like Swiggy, Zomato, and Myntra heavily rely on auto-recovery. 
If you add a shirt to your cart on Myntra and close the app, 30 minutes later your phone buzzes: *"Did you forget something? Complete your purchase before it goes out of stock!"* 

According to industry reports, sending an automated push notification within the first hour of abandonment recovers **up to 15% of lost sales**. If your average order value is ₹1,000, and 100 people abandon their cart a day, this single feature automatically generates **₹15,000 extra revenue per day** without you lifting a finger.

---

## 🛠️ How We Build It in Go (CrazyCoconut)

Because we are using a high-performance Go backend with Redis, this is surprisingly elegant to build. We don't need expensive third-party tools like Klaviyo.

### The Logic Flow:

1. **The Cart Event**: A user taps "Add to Cart" in the Flutter app. The API `POST /api/v1/cart/items` is triggered.
2. **The Redis Timer**: The Go backend adds the item to the database, but it *also* drops a record into Redis with a 30-minute expiry timer. `SET cart_abandoned:user_123 "pending" EX 1800`
3. **The Purchase (Cancellation)**: If the user completes the checkout within 30 minutes, our Checkout API deletes the Redis key. The timer is canceled.
4. **The Abandonment (Trigger)**: If the user *closes* the app without buying, the 30-minute Redis timer expires. Redis natively fires an "Expired Event" (Keyspace Notification).
5. **The Go Background Worker**: Our Go backend listens for this Redis expiration event. It wakes up a background Goroutine, checks the user's `FCMToken` in PostgreSQL, and uses the `firebase-admin-go` SDK to instantly fire a push notification: *"Your Coconut Bowl is waiting! 🥥 Tap here to checkout before it sells out."*

---

## 📝 Implementation Tasks

To add this powerful revenue engine to our backend, we need to:
- `[ ]` Enable Redis Keyspace Notifications in our server config (`config.set notify-keyspace-events Ex`).
- `[ ]` Create a Go background worker (Goroutine) in `cmd/crazycoconut/main.go` that subscribes to Redis expiry channels.
- `[ ]` Update the Cart Handlers (`POST /api/v1/cart/items`) to set/refresh the 30-minute Redis key on every addition.
- `[ ]` Update the Checkout Handler (`POST /api/v1/checkout/success`) to delete the Redis key upon success.
- `[ ]` Implement the `firebase-admin-go` messaging payload logic to actually send the notification.
