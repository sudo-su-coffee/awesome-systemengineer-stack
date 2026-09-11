# 🔌 Universal SaaS: Marketplace & Ecosystem Engineering Stack

This document defines the **Extensibility Layer** of the platform—synthesizing **Plugin Sandboxing (Wasm)**, **Industrial Webhook Action Loops**, and **Marketplace Governance**. It is the blueprint for a sovereign developer ecosystem where 3rd-party code can extend the platform's core logic safely.

---

## 🏛️ 01. Plugin Sandboxing (The Wasm Standard)

We reject the "Unsafe Script" model. 3rd-party code MUST be sandboxed.

- **WebAssembly (Wasm) Isolation**: Extensions are compiled to Wasm and executed via a high-performance runtime (e.g., **Wasmtime** or **Wasmer**) embedded in the Command Tier (T3).
- **Resource Jailing**: Every plugin is assigned a strict **CPU and Memory quota**. If a plugin enters an infinite loop or exceeds 50MB of RAM, it is instantly terminated by the platform watcher.
- **Zero-Trust I/O**: Plugins cannot access the network or filesystem directly. They must use the **Whatomate ABI (Application Binary Interface)** to request data or trigger actions via host-functions.

---

## ⚡ 02. The Extension Hub & Public API

We provide a specialized, stable contract for the "External Brain."

- **Public API Contract**: A versioned, high-performance REST/gRPC API surfaced via the **Ingress Gateway (T1)**.
- **Webhook Action Loops**: 
    - **Inbound Hook**: "Trigger" (e.g., "On Message Received").
    - **Extension Logic**: 3rd-party processing (e.g., "Translate to Spanish").
    - **Outbound Action**: "Response" (e.g., "Send Translated Message").
- **Secret Proxying**: Extensions never see the platform's core secrets. They use **Short-Lived Access Tokens (SLATs)** with scoped permissions.

---

## 📜 03. Marketplace Governance & Certification

We maintain a "Curated Excellence" marketplace, mirroring the **Zoho/Apple** standards.

1.  **Industrial Certification**: Every published plugin undergoes an automated **Static Analysis (SAST)** and a mandatory manual review of its "Intent and Permissions."
2.  **Versioning & Deprecation**: Plugins are version-pinned. Tenants can choose to update or stay on a stable legacy version.
3.  **Monetization Hooks**: Standardized logic for plugin licensing, usage-based billing, and revenue sharing for developers.

---

## 🛠️ 04. Developer Experience (DevEx) & SDKs

We empower the ecosystem with top-tier tooling.

- **The Whatomate SDK**: Lightweight libraries for Go, TypeScript, and Rust to help developers build Wasm-compatible plugins in seconds.
- **Mock Handlers**: A local testing CLI that allows developers to simulate WhatsApp webhooks and payment triggers on their local machines.
- **Documentation Sovereignty**: Automatically generated developer portals (Swagger/Stoplight) for every marketplace-exposed endpoint.

---

## 🛡️ 05. Security: The Extensibility Guard

- **Rate-Limiting Extensions**: 3rd-party plugins are rate-limited separately from the core tenant to prevent a rogue plugin from exhausting the tenant's token quota.
- **Audit Logging**: Every action taken by an extension is logged as an `ACTION_EXTENSION` event in the **Audit Tier (T7)**, providing full transparency on who sent what message.

---
*Last Updated: April 2026 | Following the Universal SaaS Engineering Standard.*
