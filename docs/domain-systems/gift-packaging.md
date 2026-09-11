# Premium Gift Packaging System

Because CrazyCoconut sells unique, handcrafted items, a large percentage of orders will be purchased as gifts. Offering a Premium Gift Packaging option during checkout is an incredibly high-margin feature.

## 1. The UX Flow
1. User adds a Handcrafted Coconut Shell Lamp to their cart.
2. In the Cart screen, the Flutter app displays an upsell toggle: **"🎁 Make it a Gift (+₹99)"**.
3. If toggled, a text box appears allowing them to type a personalized 140-character message.

## 2. The Backend Implementation
We simply update the `Order` and `Cart` models in Go to support these variables:
- `is_gift` (boolean)
- `gift_message` (string)
- `gift_fee` (float64)

When calculating the final total, the Go backend dynamically adds the `gift_fee` (e.g., ₹99) to the total invoice.

## 3. The Fulfillment Pipeline (Admin)
When an order hits the Admin Panel, if `is_gift == true`:
1. The order row is highlighted in **Gold** so the packer knows instantly.
2. The Go backend dynamically generates a beautiful PDF packing slip.
3. Instead of printing a boring invoice with prices, it prints a stylized "Gift Tag" containing the exact `gift_message` the user typed, and hides all pricing information!
4. The artisan packs the item in premium wrapping paper, attaches the printed gift tag, and ships it.

## 4. The Business Value
Gift wrapping usually costs the vendor ₹15 in materials, but users happily pay ₹99 for the convenience. This adds pure profit margin to the order and creates a deeply emotional unboxing experience for the recipient (who then downloads the app to buy more things!).
