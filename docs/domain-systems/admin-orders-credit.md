# Admin-Initiated Orders & B2B Credit System

In standard e-commerce, the customer must always initiate the order on their phone. However, in an Enterprise/ERP system, you need the flexibility to create orders manually from the backend. 

Our Go API will support a dedicated endpoint: `POST /api/v1/admin/orders/create`

## 1. Why do we need this?
- **Phone Orders**: A customer calls support because their app isn't working, but they desperately want to buy a Coconut Lamp. The admin can open the dashboard, select the user's profile, add the lamp, and generate the order manually.
- **Wholesale / B2B Deals**: A corporate client wants to buy 500 Coconut Bowls for a corporate event. They aren't going to use the mobile app to checkout 500 items via Razorpay. The admin generates this massive order manually.

## 2. The Credit System (Net-30 / B2B)
When the admin creates the order via the Go API, they can bypass the standard payment gateway requirements. 

We will update the `PaymentStatus` field in our PostgreSQL database to support:
- `unpaid`
- `paid`
- `cod`
- `credit` (New!)

If the admin selects `credit`, the Go backend locks the inventory and ships the item, but marks the user's account with a **Negative Balance / Outstanding Invoice**. 
This is the equivalent of a "Net-30" business term. The corporate client receives the 500 bowls and pays via Bank Wire Transfer 30 days later. Once the money hits the bank, the admin changes the status from `credit` to `paid`.

## 3. The "Payment Link" Fallback
If the admin creates an order for a normal customer over the phone, the Go backend can integrate with Razorpay to generate a **Payment Link**.
1. Admin creates the order.
2. Go backend generates `https://rzp.io/i/xxxxxx`.
3. Go automatically sends an SMS/Email to the customer: *"Your order was created by CrazyCoconut Support! Click here to pay securely."*
4. The user clicks, pays, and the order is instantly flipped to `paid`.
