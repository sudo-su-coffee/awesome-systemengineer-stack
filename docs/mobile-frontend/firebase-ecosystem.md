# The Firebase Ecosystem (All-in-One Masterplan)

While our custom Go backend and PostgreSQL database handle the complex business logic, commerce transactions, and inventory, we use the **Firebase Ecosystem** as our invisible infrastructure layer to handle everything related to App Health, Analytics, and Client-Side Delivery.

Here is exactly how CrazyCoconut utilizes every tool in the Firebase suite:

## 1. Firebase Remote Config (Dynamic UI & Toggles)
As discussed, this acts as the control center for the mobile app.
- **Usage**: Pushing new UI strings, changing Lottie animations dynamically, updating marketing banners, and managing feature flags (e.g., turning off Apple Pay remotely if it's broken).

## 2. Firebase Cloud Messaging (FCM) (Silent WebSockets)
We do not use battery-draining WebSockets on the mobile app. We use FCM.
- **Silent Data Messages**: When the Go backend detects an order status change, it uses the `firebase-admin-go` SDK to ping the user's phone in the background. The app wakes up, updates the UI, and goes back to sleep.
- **Marketing Pushes**: Used for our Abandoned Cart Recovery system and Flash Sale announcements.

## 3. Firebase Crashlytics (App Health)
When the app crashes on a user's phone, they rarely report it. They just delete the app.
- **Usage**: Crashlytics automatically logs every single fatal and non-fatal error in the Flutter app. It groups them by phone model (e.g., "This crash only happens on Samsung S21s") and gives the exact line of Dart code that caused it.

## 4. Google Analytics for Firebase (User Behavior)
We do not build custom tracking in Go; we let Firebase handle the massive scale of analytics.
- **Event Tracking**: We track events like `add_to_cart`, `checkout_started`, and `purchase_completed`.
- **Funnels**: We can see exactly where users drop off. If 1,000 people hit `checkout_started` but only 200 hit `purchase_completed`, we know there is friction on the payment screen.

## 5. Firebase Performance Monitoring
- **API Tracking**: It automatically measures how long our Go backend takes to respond to the Flutter app. If the `GET /products` API suddenly takes 4 seconds instead of 100ms, Firebase alerts us.
- **Screen Rendering**: It tracks "Slow rendering frames" in Flutter to ensure the app is running at a smooth 60 FPS on all devices.

## 6. Firebase Dynamic Links (Deep Linking)
- **Usage**: When a user shares a product with their friend on WhatsApp, they send a Firebase Dynamic Link (e.g., `link.crazycoconut.com/bowl`).
- If the friend has the app, it opens directly to the product page.
- If they *don't* have the app, it sends them to the App Store, and *after* they install, it automatically opens the exact product page they clicked!
