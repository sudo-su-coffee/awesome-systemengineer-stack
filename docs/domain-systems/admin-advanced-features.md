# Advanced Admin & Operational Features

Building on top of the "Admin-Initiated Orders", here are the final 4 enterprise-grade features that massive platforms like Shopify and Amazon use to run their operations smoothly. 

These are built directly into the Go backend and Postgres database.

## 1. RBAC (Role-Based Access Control)
As your company grows, you will hire staff. You cannot give everyone the "Super Admin" password.
- **The Structure**: Our Go backend will enforce strict JWT roles.
- **SuperAdmin**: Can see total revenue, delete users, and change bank details.
- **SupportAgent**: Can only view orders, issue refunds, and initiate returns. Cannot see total company revenue.
- **Packer**: Can only see the list of items to pack for the day and print AWB labels. They cannot see the price of the items.

## 2. The RMA & Returns Engine (Reverse Logistics)
Handling returns manually is a nightmare. We build a robust RMA (Return Merchandise Authorization) state machine in Go.
1. User clicks "Return Item" in the app (uploading a photo of a broken bowl).
2. The Go backend alerts the `SupportAgent`.
3. The Agent clicks "Approve". Go instantly fires a webhook to Delhivery to initiate a **Reverse Pickup** from the customer's house.
4. **Full vs Partial Refunds via UI**: As an admin, you can select whether to issue a **Full Refund** or a **Partial Refund** directly from the dashboard UI.
5. **Wallet vs Source Refunds (The Gateway Fee Problem)**: By default, payment gateways (like Razorpay) do not refund the 2% transaction fee when you initiate a refund—you (the merchant) eat that loss. To bypass this, our Go backend pushes for **Wallet Refunds** (Coconut Coins). However, if the user insists on a "Refund to Source", the admin simply clicks a button in the UI, and the Go backend automatically calculates and processes the refund back to the user's bank, minus any applicable return shipping fees.

## 3. Dynamic Pricing & B2B Segmentation
Not all users should see the same price.
- **Retail Users**: Open the app and see the Coconut Bowl for ₹500.
- **Wholesale/B2B Users**: A corporate gifting company logs in. The Go backend detects their `account_type = "B2B"` and instantly dynamically calculates a 30% discount on all items. They see the exact same bowl for ₹350, entirely automatically.

## 4. "Subscribe & Save" (Recurring Billing)
Just like Amazon's subscription model.
- If you sell consumables (like handcrafted scented coconut candles), users can select "Deliver every 30 days".
- The Go backend uses **Razorpay eMandates / UPI AutoPay**. 
- A Go background worker runs every night. If it sees a subscription due tomorrow, it automatically pings the bank, deducts the money, creates the order, and drops it into the Packer's queue—completely passively generating revenue!
