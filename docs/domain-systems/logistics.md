# Logistics & AWB Management Architecture

Third-party logistics aggregators (like Shiprocket) charge high fees for API integration. For a highly profitable marketplace, we want the ability to handle shipping our own way.

## 1. Manual AWB & Custom Generation
Instead of relying on a costly third-party API to generate Airway Bills (AWBs), our Go backend allows for total flexibility:

1. **Manual Entry**: When a vendor packs an order and takes it to the local post office (India Post) or DTDC, they receive a tracking number. The vendor simply logs into our Admin Panel and types the tracking number into the order.
2. **Custom AWB Generation**: If we run our own delivery fleet in a specific city, our Go backend can instantly generate a unique internal AWB (e.g., `CC-992384-BLR`) and use a Go library to generate a Code128 PDF Barcode. The packer prints this out and slaps it on the box.

## 2. The Tracking Flow
When an AWB is entered (manually or generated):
1. The Go backend updates the `Order` status to `Shipped`.
2. The `awb_number` and `courier_partner` fields are saved to PostgreSQL.
3. The Go backend fires a silent FCM push notification to the user's phone.
4. The user taps the notification, and the Flutter app opens the "Order Tracking" screen displaying the AWB.

## 3. Webhook Integrations (Future-Proofing)
If we ever negotiate a massive deal with Delhivery or BlueDart directly:
Our Go backend is ready. We simply add a `POST /api/v1/webhooks/delhivery` endpoint. When Delhivery scans the package at their warehouse, they ping our Go server, and we instantly push that live location update to the customer via Redis Pub/Sub!
