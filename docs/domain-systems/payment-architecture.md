# Payment Gateway Architecture

In modern e-commerce apps like CrazyCoconut, relying on a single payment gateway can lead to massive revenue loss if that gateway goes down. We must architect our Go backend to support **Payment Routing**.

## 1. The Primary Aggregator: Juspay
For a large-scale Indian app, **Juspay** is the gold standard (used by Swiggy, Amazon India, Cred).
- **Why?** Juspay is not a payment gateway; it is a *payment router*. It sits on top of Razorpay, PayU, Cashfree, and Paytm.
- **Dynamic Routing**: If Razorpay's UPI success rate drops below 70% during a flash sale, Juspay automatically routes the user's payment to PayU without the user ever noticing. This increases total payment success rates by 5-10%.

## 2. Direct Integration (Fallback/Alternatives)
If you prefer not to use an aggregator like Juspay, we will implement **Razorpay** as the primary gateway, with **PayU** as a fallback.

### How it works in our Go Backend:
1. User clicks "Pay ₹1000".
2. The Go backend checks the `payment_gateways` section in `config.toml` or Firebase Remote Config.
3. If Remote Config says `"primary_gateway": "razorpay"`, the Go backend generates a Razorpay `order_id` and sends it to the Flutter app.
4. If Razorpay's API is down (returns 500 error), the Go backend instantly falls back and generates a PayU `txn_id` instead.

## 3. The "One-Click" Tokenization
To support our One-Click Checkout feature, we use **Card Tokenization** (compliant with RBI guidelines).
When a user saves a card on Razorpay, Razorpay gives our Go backend a secure token (e.g., `token_xyz`). Next time they buy, we just send `token_xyz` to the gateway to charge them instantly without re-entering details.
