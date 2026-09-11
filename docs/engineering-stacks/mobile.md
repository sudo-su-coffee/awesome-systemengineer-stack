<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:43cea2,100:185a9d&height=200&section=header&text=Mobile%20Engineering&fontSize=60&fontAlignY=35&desc=Universal%20SaaS%20Mobile%20Architecture" width="100%" alt="Mobile Engineering Stack Banner"/>
</div>

# 📱 Universal SaaS: Mobile Engineering Stack (BlackLoverTech Edition)

This document defines the high-performance mobile architecture for the platform, adopting the **Zerodha "Mechanical Sympathy"** model and **Zoho's minimalist feature-density philosophy**. We prioritize raw performance, UI consistency, and binary efficiency over "developer convenience" (JS frameworks).

---

## 🛠️ The Core Decision: Why React Native was a "Miss"

Following the **Zerodha Technical Post-Mortem** strategy, we explicitly avoid "The Hype Cycle." We choose tools that are proven, deterministic, and respect human hardware interfaces.

1.  **The "Bridge" Bottleneck**: React Native relies on a JS-to-Native bridge. In a data-heavy app, the serialization overhead between the JavaScript thread and the Main UI thread causes dropped frames and "jank."
2.  **Fragmented Rendering**: Because React Native uses platform-native views, the app looks and behaves differently across 10,000+ different Android manufacturer skins (MIUI, ColorOS, etc.).
3.  **Upgrade Fatigue**: The high dependency on the internal JS Engine (Hermes/JSC) and frequent breaking changes in the native bridge makes long-term maintenance an operational nightmare.

---

## 🚀 The Native-Performance Stack

We adopt a **"Macro-Service" Mobile Architecture**: **Flutter** for the primary UI engine, specialized **Native (Kotlin/Swift)** for platform-deep systems, and **gRPC** for the data layer.

### 1. Primary UI Engine: [Flutter](https://github.com/flutter/flutter) (Dart/Impeller)
*   **Why**: Unlike React Native, Flutter renders directly to the GPU using the **Impeller** engine (Skia legacy). It paints every pixel, ensuring **100% UI consistency** across all devices.
*   **Performance**: AOT (Ahead-of-Time) compilation to ARM machine code. Zero JS bridge overhead. 
*   **Mechanical Sympathy**: Flutter handles its own layout and painting, effectively bypassing the overhead of the Android View system.

### 2. High-Performance Data Layers
We utilize a multi-rail communication strategy depending on the payload complexity.

*   **⚡ [gRPC / Protobuf](https://github.com/grpc/grpc) [Go/Dart]**: The primary data rail for all real-time platform operations. Binary serialization ensures the lowest possible latency and battery consumption.
*   **🕷️ [flutter_rust_bridge](https://github.com/fzyzcjy/flutter_rust_bridge) [Rust/Dart]**: **The Performance Elite Standard**. We use this to run heavy computational logic (encryption, data parsing, media transcoding) in a separate Rust thread, keeping the Dart UI thread 100% jank-free.
*   **🕸️ [GraphQL](https://github.com/graphql/graphql-spec) [Go/Clojure]**: Utilized specifically for **Analytics and Reporting** features through **[ClickHouse GraphQL](https://github.com/ranjanrak/clickhouse-graphql-go)**.
*   **🗄️ [Isar Database](https://github.com/isar/isar) [Rust/Dart]**: Our local persistence engine. Built in Rust for asynchronous, ACID-compliant storage.
*   **🌳 [fpdart](https://github.com/SandroMaglione/fpdart) [Dart]**: Functional programming patterns (Option, Either, Task) to eliminate null-checks and handle business logic errors as first-class citizens (Zerodha internal preference).

### 3. Native Platform Hardening (Kotlin & Swift)
While Flutter handles the UI, we utilize **Platform Channels** to write direct Kotlin (Android) and Swift (iOS) code for:
*   **[GRPC Notification System](https://github.com/nammayatri/notification_service) [Go]**: Our internal high-throughput dispatcher.
*   **📦 FCM (The Delivery Rail)**: While our logic is gRPC-based, we utilize **FCM (Firebase Cloud Messaging)** as the "dumb pipe" for delivery to Android/iOS. We keep this integration minimal and secure, using it only as a wake-up signal for the gRPC persistent connection.
*   **💳 [Hyperswitch Mobile SDK](https://github.com/juspay/hyperswitch-sdk-ios) [Go/Rust/Native]**: The unified payment interface. Instead of embedding multiple payment SDKs (Razorpay, Stripe), we use the single Hyperswitch SDK to route transactions dynamically at the edge.
*   **🔐 Native Hardening**: 
    *   **SSL Pinning**: Mandatory pinning of gRPC certificates to block MITM attacks. 
    *   **Biometric Guard**: Hardware-backed biometric authentication for sensitive account actions.
*   **Background Relays**: Maintaining long-lived websocket connections in persistent background services.

---

## 🏗️ State Management & UI Architecture

We prioritize **Simplicity over Reactivity**. We avoid complex "magic" frameworks in favor of predictable, testable state machines.

*   **💾 State Management**: **[Riverpod](https://github.com/rrousselGit/riverpod) [Dart]**. Chosen for its compile-time safety and lack of dependency on the BuildContext (Mechanical Sympathy). Allows us to manage complex gRPC stream states cleanly.
*   **🎨 Design Tokens (Zoho-Style)**: We use a **Custom Theme Engine** built on top of **Material 3**. 
    *   **Palette**: Void Black (`#000000`) for dark mode, Zoho Amber for core actions.
    *   **Minimization**: We remove all decorative blurs and heavy gradients in favor of sharp, fast-painting solid boundaries.
*   **📦 Local Persistence (Offine-First)**: 
    *   **[Hive](https://github.com/hivedb/hive) [Dart]**: Blazing fast key-value storage for small caches and settings.
    *   **[Isar](https://github.com/isar/isar) [Rust/Dart]**: Relational NoSQL for the primary message inbox and contact directory. We implement a **Write-Through Cache** strategy: all API data is persisted here before being displayed.
*   **🏗️ Mobile Shell Architecture**: 
    - **Cellular App Shell**: For larger screens (Tablets/Foldables), we use the **Global Sidebar** pattern. For mobile, a bottom-persistent command bar.
    - **Sheet Navigation**: New contexts slide in as "Sheets" from the bottom or right, maintaining the Zoho-like focus.

### 🏗️ Server-Driven UI (SDUI) & Dynamic Rendering
To maintain "Sovereign Control" over the UI without App Store bottlenecks, we adopt a **Hybrid SDUI** model.
- **Mechanism**: Flutter native components mapped to server-sent JSON schemas.
- **Asset Integration**: High-fidelity Lottie (JSON) and Vector (SVG) assets injected directly from the backend.
- **Industrial Titans**: Adopting the **Airbnb "Ghost Platform"** approach (Section-based UI) and **Google Pay** logic mesh for dynamic fintech flows.
- **Logic Layers**: Utilizing separate **Deluge-style DSLs** or **WASM behavior injection** for complex client-side interactions.

### 🛠️ Engineering Mode & Local Observability: 
    *   **Schema First**: All endpoints are defined in `.proto` files (`/proto/service.proto`).
    *   **Generation**: We use `protoc` with the Dart plugin to generate immutable service clients and data models.
    *   **Middleware**: Every API call automatically attaches the `X-Org-ID` and `X-Account-ID` from the secure hardware keystore.

---

## 🛠️ Engineering Mode & Local Observability
To debug scaling issues in the field, we built a custom logging layer.

*   **🪵 Offline Log Access**: We use a custom **[onelog](https://github.com/francoispqt/onelog)**-style logger in Dart that pipes all debug/error logs to a local **Isar** table (`mobile_logs`).
*   **📡 Inspection Portal**: A hidden "Engineering UI" (triggered by a secret tap sequence) allows engineers to:
    - View raw gRPC request/response payloads (buffered locally).
    - Access local storage (Hive/Isar) directly for state inspection.
    - Export local logs as a `.zip` file for remote debugging.
*   **🔌 Device Diagnostics**: Using **[Anpec](https://github.com/zerodha/anpec)** to visualize real-time CPU/Memory usage and frame drops (Jank) within the app.

---

## 📊 Mobile Observability & Crisis Management

Scaling to millions of users (like Zerodha's Kite app) requires extreme visibility into the field.

*   **📉 Error Tracking**: **[Sentry (Self-Hosted)](https://github.com/getsentry/sentry) [C++/Python]**. We ensure all mobile crashes and Dart exceptions are captured in our private Sentry instance with full stack traces.
*   **📈 Product Analytics**: **[Umami](https://github.com/umami-software/umami) [Node]**. Privacy-first, lightweight analytics to track feature usage without leaking user PII to Google/Meta.
*   **📡 Event Ingestion**: **[Logchef](https://github.com/mr-karan/logchef) [Go]**. Minor UI events are pushed to a Logchef endpoint and stored in **ClickHouse** for long-term behavior analysis.
*   **🔌 Continuous Profiling**: **[Pyroscope](https://github.com/grafana/pyroscope) [Go]**. Integrating the Pyroscope SDK to find CPU/Memory leaks in real devices during high-load periods.

---

## 🏛️ The FOSS Mobile Toolkit (Zerodha Engineering Index)

We integrate Zerodha's battle-hardened internal mobile tools to ensure observability and developer experience (DX).

*   **⚡ [Recharge](https://github.com/ajinasokan/recharge)**: A specialized hot-reload tool for high-performance development cycles.
*   **🧪 [Autopilot](https://github.com/ajinasokan/autopilot)**: Black-box UI testing tool. Essential for automating end-to-end (E2E) testing of flows without manually interacting with physical devices.
*   **🖼️ [Tagflow](https://github.com/zerodha/tagflow)**: A high-performance Flutter package to render HTML/rich-text as native Flutter widgets without using expensive WebViews.
*   **📊 [Anpec](https://github.com/zerodha/anpec)**: Android performance classification library used to identify low-end devices and dynamically degrade UI (e.g., turning off heavy animations) to maintain performance.
*   **📦 [Stuffbin](https://github.com/zerodha/stuffbin)**: Pattern for embedding static assets directly into the binary.
*   **🎭 [Lottie-Flutter](https://github.com/xvrh/lottie-flutter)**: Our standard for dynamic animations. We ship Lottie JSONs over the wire via SDUI to avoid APK bloat for seasonal/promotional UI.
*   **📐 [Flutter_SVG](https://github.com/dnfield/flutter_svg)**: Supporting the `.vec`/SVG standard for resolution-independent icons and illustrations.

---

## 🚀 CI/CD, Automation & Over-the-Air (OTA)

To maintain the **Zerodha-style agility** (shipping fast without compromising stability), we utilize a high-performance automation pipeline that bypasses traditional App Store bottlenecks.

*   **⚡ [Shorebird.dev](https://shorebird.dev/) (Code Push)**: The definitive "over-the-air" update engine for Flutter. It allows us to push critical bug fixes and patch logic directly to user devices **without an App Store review**. This solves the #1 historical weakness of Flutter compared to React Native.
*   **🏎️ [Fastlane](https://github.com/fastlane/fastlane) [Ruby]**: The industry standard for mobile automation. **100% Open Source and Local**. We use it to automate screenshots, metadata, and binary signing on our internal hardware.
*   **🏗️ Self-Hosted Runner Clusters**: Instead of cloud builders, we utilize **GitLab Runners** deployed on our internal **Nomad** cluster. This ensures that every mobile build is reproducible, fast, and stays within our private network.
*   **📦 [oore.build](https://github.com/devaryakjha/oore.build)**: Our unified **Self-hosted, Flutter-first mobile CI platform**.

### ⚔️ The Sovereign Decision: OTA vs. SDUI
We distinguish between **Code-Push (OTA)** and **Schema-Driven UI (SDUI)**:
- **OTA (Shorebird)**: Reserved for critical logic patches and security fixes. High risk of store policy friction.
- **SDUI**: Primary rail for UI updates and feature discovery. 100% compliant and instantaneous with zero download delay.

---

## 🛡️ The Evolution of React Native (A Post-Bridge World)

While our stack defaults to Flutter for its "painless" performance, a high-performance systems engineer must understand how **React Native (RN)** has evolved to address the "Miss" described in the Zerodha post-mortem. 

The "New Architecture" (2024+) has fundamentally changed the internal engine of React Native:

1.  **JSI (JavaScript Interface)**: The biggest advancement. It completely replaces the old asynchronous, JSON-serialized "Bridge." JS can now hold a reference to C++ host objects and invoke methods **synchronously**. This mimics the performance of a native C++/Rust bridge.
2.  **Fabric Rendering**: A new UI manager that is also synchronous. It allows for better integration with host platform features like accessibility and RTL, and eliminates the "white screen" flashes during high-intensity scrolls.
3.  **TurboModules**: A new way to define native modules that are loaded lazily. This significantly improves app startup time by not loading every native module at the beginning.
4.  **[react-native-skia](https://github.com/Shopify/react-native-skia) [C++]**: Shopify's engine that brings the high-performance 2D graphics engine from Flutter to React Native, allowing RN apps to achieve Flutter-like UI complexity and smoothness.
5.  **[MMKV](https://github.com/mrousavy/react-native-mmkv) [C++]**: A hyper-fast, key-value storage framework (initially by WeChat) that replaces the slow and asynchronous AsyncStorage.
6.  **[react-native-reanimated](https://github.com/software-mansion/react-native-reanimated) [C++]**: Declarative animations that run on the UI thread via **Worklets**, bypassing the JS thread bottlenecks.

**Conclusion**: React Native is no longer "the slow one," but it still carries the complexity of managing a dual-runtime (JS + Native). For the platform, we favor the **Single-Runtime (Dart/Skia)** simplicity of Flutter, but we respect the **JSI-synchronous** breakthrough and the **Worklet-driven** animation model of the modern RN ecosystem.

---

## 🏗️ Hybrid Systems: Kotlin Multiplatform (KMP) & Rust-in-Mobile

For the most critical business logic (e.g., protocol encryption, multi-tenant session reconciliation), we occasionally step out of the UI framework entirely:

*   **[KMP](https://github.com/JetBrains/kotlin-multiplatform-mobile) [Kotlin]**: Shared business logic across Android and iOS while maintaining raw native performance. Used when Flutter's platform channels introduce too much latency for high-frequency data handling.
*   **Rust (System-Core)**: All sensitive cryptography and data-parsing logic is written in **Rust** and shared between the Go backend and the Mobile apps via **FFI (Foreign Function Interface)**. This ensures 100% logic parity and memory safety across the entire stack.

---

## 📐 Product Philosophy: Design for Disengagement

In line with Zerodha's philosophy, the mobile codebase is treated as a high-precision instrument.

1.  **Speed as a Feature**: Sub-millisecond UI responsiveness is prioritized over decorative animations. If a transition takes more than 100ms, it is removed.
2.  **Notification Sanity**: We use the **gRPC Notification System** to deliver high-signal alerts only. We avoid engagement-driving "nudges" that clutter the user's life.
3.  **No-Infinite-Scroll**: Every screen has a clear "Done" state. We want users to get in, manage their automation, and get out.

---

## 📦 Mobile Operational Registry (Self-Hosted Only)

To maintain 100% data sovereignty, we avoid third-party SaaS for core mobile infrastructure.

*   **🔐 Identity (Auth)**: **[Zitadel](https://github.com/zitadel/zitadel) [Go]** or **[Authentik](https://github.com/goauthentik/authentik) [Python/Go]**. Self-hosted OIDC/SAML providers for enterprise SSO and mobile login.
*   **🗺️ Maps & Location**: **[MapLibre](https://github.com/maplibre/maplibre-gl-js) [C++/JS]**. Vector map engine used as a FOSS alternative to Google Maps SDK to avoid tracking and usage costs.
*   **🪵 Unified Logging**: **[onelog](https://github.com/francoispqt/onelog) [Go]** strategy mirrored in Dart. All mobile logs are structured (JSON), leveled, and batched before being shipped to **Logchef/ClickHouse**.

---

## ✅ Mechanical Sympathy Checklist for Mobile

- [ ] **No Reflection**: Avoid any package that uses `dart:mirrors` or heavy reflection-based JSON parsing (Use `easyjson`/`json_serializable`).
- [ ] **Thread Separation**: All non-UI work (Parsing, DB queries) must happen on an **Isolate** (Flutter) or a **C++ Worklet** (RN).
- [ ] **Binary Size**: Use `stuffbin` to compress assets. Regularly audit `apk analyzer` to keep the install size under 20MB.
- [ ] **Offline Sync**: Is the app usable during a flight? Use **Isar/Hive** for aggressive optimistic UI.

---

## 🎯 Mobile Operational Mandate

1.  **Single Binary Logic**: Avoid dynamic code pushes (like Codepush). We value the integrity of the signed binary.
2.  **SSL Pinning**: Mandatory SSL pinning for all gRPC traffic to prevent Man-in-the-Middle (MITM) attacks on enterprise WhatsApp data.
3.  **Zero-Heavy-Dependencies**: Every third-party plugin must be audited for size and performance. We prefer writing a small 100-line native bridge over importing a 5MB bloatware library.
4.  **Offline-First Cache**: Local persistence layer using **[Hive](https://github.com/hivedb/hive)** (NoSQL/Binary) or **[SQLite](https://github.com/sqlite/sqlite)** for instantaneous UI responsiveness even under flaky network conditions.

---

## 📁 Repository Structure
```bash
/mobile
  /lib          # Core Flutter (Dart) Logic
  /android      # High-performance Kotlin Bridges + GRPC Handlers
  /ios          # High-performance Swift Bridges
  /proto        # Shared gRPC Definitions (Source of Truth)
  /scripts      # Autopilot & Recharge automation
  /tools        # Sovereign Exploration (LiveContainer, PulseAPK)
```

---

## 🚀 14. Advanced Native Exploration & Security (Sovereign Tools)

To maintain absolute control over the mobile environment and perform deep security audits, the following high-agency tools are integrated into the research workflow:

- **iOS Sandbox Bypass:** **[LiveContainer](https://github.com/LiveContainer/LiveContainer)** — Run iOS apps without actually installing them! Essential for testing and isolating third-party app behaviors in a non-persistent environment.
- **APK Security & Decompilation:** **[PulseAPK](https://github.com/deemoun/PulseAPK)** & **[PulseAPK-Core](https://github.com/deemoun/PulseAPK-Core)** — A cross-platform tool for working with APK files (Decompilation, Analysis, Building). Features a WPF frontend for `apktool` and `uber-signer` with live decompilation output and smali analysis.

---
*Last Updated: April 2026 | Following the BlackLoverTech "Build to Learn" Mandate.*
