# App Updates & "Swiggy Minis" Architecture

We have shifted massive amounts of UI logic to Firebase Remote Config (Lottie animations, strings, banners, colors). However, there is still a hard line between what can be updated remotely and what requires a hard Play Store/App Store update.

## 1. When do you ACTUALLY need a Play Store Update?
Since Apple and Google strictly prohibit apps from downloading new compiled code (Dart/Swift/Kotlin) after installation, you **must** push a real App Store update when:
- **Adding a New Flutter Package**: If you want to add Google Maps, Stripe SDK, or a Barcode Scanner, the native C++/Java/Objective-C libraries must be compiled into the binary.
- **Brand New Screens with Unique Logic**: If you build a completely new "Video Feed" page (like TikTok) that requires complex scroll controllers and new video players, it must be an update.
- **Major OS Changes**: When iOS 18 or Android 15 releases and changes how permissions work.

*Rule of Thumb: If it requires writing new Dart logic or adding a new `pubspec.yaml` dependency, it needs an update. If it's just changing colors, text, or layout orders, use Firebase.*

---

## 2. How did Swiggy build their "Plus Icon / Mini App" Overlay?
During Valentine's Day (Feb 14) or IPL, Swiggy drops a floating "Plus" button or a banner that opens a crazy interactive game (Spin the Wheel) or a special "Chocolate Delivery" flow as an overlay inside the app.

**How do they build this without updating the app?**
They use a concept called **Micro-Frontends (or Mini Apps)**. There are two main ways to do this in Flutter:

### Method A: Micro-Frontends & SSR (The Swiggy Engineering Approach)
According to Swiggy's actual engineering blogs ("Swiggy Bytes"), they heavily utilize a **hybrid architecture** to pull this off:
1. **The Core App**: Swiggy's main mobile shells are built using a mix of Native (Kotlin/Swift) and **React Native**.
2. **The Web (Minis)**: For rapid campaigns (like Feb 14), their web team builds the UI using **React 18** and **Server-Side Rendering (SSR)**.
3. **The Overlay**: When you tap the Plus icon, the native app opens a highly optimized WebView. Because they use SSR, the HTML is streamed instantly to the phone, eliminating the white loading screen.
4. **The Design Language System (DLS)**: Swiggy uses a shared DLS across Native, React Native, and React Web. This means the buttons in the WebView look *exactly* like the native buttons in the app, so the user has no idea they just opened a website!

**Why it's genius**: The web team can update the game 50 times a day. The app doesn't need a single update, and SSR ensures it loads as fast as native code.

### Method B: Server-Driven UI (SDUI)
If it's not a game, but a new shopping flow (like a special Valentine's Checkout), they use SDUI.
Instead of sending a web page, the backend sends a massive JSON file that describes the UI:
```json
{
  "type": "Column",
  "children": [
    {"type": "Text", "value": "Happy Valentine's Day!", "color": "#FF0000"},
    {"type": "Button", "action": "buy_chocolates"}
  ]
}
```
The mobile app has a pre-built parser that reads this JSON and draws the native widgets dynamically on the screen.

## How we can do this in CrazyCoconut:
For massive seasonal campaigns (like Diwali Games), we should use **Method A (WebViews)**.
1. We add a `floating_campaign_url` to our Firebase Remote Config.
2. If the URL is not empty, the Flutter app shows a floating action button.
3. When tapped, it opens that URL in a full-screen or bottom-sheet WebView.
4. You can build the game/campaign in standard HTML/JS, host it on Vercel, and plug it right into the app!
