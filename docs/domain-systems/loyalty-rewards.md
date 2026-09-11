# Loyalty & Gamification System (Coconut Coins)

To increase Customer Lifetime Value (LTV) and ensure users keep coming back to CrazyCoconut instead of Amazon, we implement a custom gamification engine in the Go backend.

## 1. The Economy ("Coconut Coins")
Every user has a wallet balance of "Coconut Coins". 1 Coin = ₹1.
Coins are stored in a dedicated PostgreSQL table (`user_wallets`) tightly linked to their profile.

## 2. Earning Coins
The Go backend automatically awards coins based on specific user actions:
- **Purchases**: 5% cashback on all orders (e.g., Spend ₹1000, get 50 Coins).
- **Referrals**: User A shares their invite link. User B installs and makes their first purchase. The Go backend automatically credits 100 Coins to User A via a Redis queue.
- **Reviews**: Leave a photo review of a product, get 10 Coins.

## 3. Burning Coins
During checkout, the Go API calculates the maximum number of coins the user can apply.
- If a user has 200 coins, and the cart total is ₹1000, they can apply their coins and pay only ₹800.
- **The Psychology**: The user feels like they got a massive discount, but we guaranteed a returning customer.

## 4. Flash Expirations (FOMO)
Coins shouldn't last forever. Our Go backend runs a nightly Cron job. If a user's coins are expiring in 3 days, Go sends a targeted FCM Push Notification: *"You have ₹200 worth of Coconut Coins expiring soon! Use them now!"* This creates massive FOMO and drives instant sales.
