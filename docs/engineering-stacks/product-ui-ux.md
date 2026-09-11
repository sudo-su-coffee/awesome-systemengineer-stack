# 💠 Universal SaaS: Product UI/UX Engineering Stack

This document defines the **Industrial Interface Contract**—synthesizing **Zoho's minimalist SaaS excellence** with **Zerodha's engineering methods**. It is the single source of truth for the **Vue.js 3** frontend stack.

---

## 🏗️ 01. The 4-Side Shell Architecture (Zoho Blueprint)

We utilize a **4-Sided Immutable Chrome** model. The shell frames remain persistent to minimize **Cumulative Layout Shift (CLS)** and browser reflows, ensuring sub-second operational continuity.

### The Shell Components
1.  **Top Command Bar (48px)**: 
    - **⌘K Global Search**: Fuzzy-search spotlight for all entities (built with `Pinia` + `TanStack Query`).
    - **Context Switcher**: Multi-tenant/Org state management.
2.  **Left Icon Rail (52px)**: 
    - **Module Nav**: Icon-only vertical rail (Inbox, Contacts, Automation).
    - **Active Indicator**: 2px amber left-border using Vue's `router-link-active`. 
3.  **Right Context Rail (64px)**: 
    - **Mini-Navigation**: Iconic shortcuts for supplementary context (Section index, System Health).
    - **Detail Drawer**: Slide-in panel for deep context (CRM history, internal notes) without changing the main URL.
4.  **Bottom Status Bar (32px)**: 
    - **Ambient Awareness**: Real-time display of API latency, Redis health, and resource quotas.

---

## 🎨 02. Design Language: "Mechanical Sympathy"

We prioritize speed and hardware efficiency. Every pixel must be justified by utility.

### Visual Principles
- **No-Spinner Mandate**: Avoid traditional loading spinners. Prefer **Industrial Skeleton Placeholders** (CSS shimmering on `--surface-2`).
- **Zero-Jank Scrolling**: Virtualization is mandatory for lists > 100 rows (using `@vue-flow` or `vue-virtual-scroller` concepts).
- **Void Aesthetic**: Base background `#080a0f` to reduce agent eye-strain during 8-hour workflows.

### Industrial Typography (Self-Hosted Only)
- **Standard**: **IBM Plex Sans** (Headings/Body) and **IBM Plex Mono** (Data/IDs).
- **Infrastructure**: All font files MUST be hosted locally in `/src/assets/fonts/`. Remote CDNs are strictly prohibited to ensure 100% availability and sub-millisecond initial paint.

---

## 📦 03. Sanctioned Frontend Kernel

We only allow high-performance, minimalist FOSS packages.

| Category | Package | Industrial Role |
|---|---|---|
| **Core** | `vue` (3.x) | Composition API (`<script setup>`) + TypeScript. |
| **State** | `pinia` / `vue-router` | Atomic stores + State-serialized in URL. |
| **Data** | `@tanstack/vue-query` | Global cache validation, atomic re-fetching. |
| **UI Core** | `reka-ui` / `vue-use` | Headless accessibility + reactive utilities. |
| **Icons** | `lucide-vue-next` | Optimized SVG icon set. |
| **Validation** | `vee-validate` + `zod` | Deterministic schema validation. |

---

## ⚡ 04. Performance & Bandwidth Engineering

Zero-jank and zero-waste are the mandates.

### HTTP Efficiency (ETags & 304)
To maintain the "Zerodha-grade" sub-100ms navigation, we leverage **Browser Cache Validation**:
- **ETag Validation**: Every GET request for static or semi-static data (e.g., Contact JSON, Template lists) must be served with an ETag (hash).
- **HTTP 304 (Not Modified)**: On re-navigating to the same route, the browser sends an `If-None-Match` header. The server returns a **304** instead of the full payload if data hasn't changed.
- **Impact**: Zero bandwidth waste. Immediate UI hydration from local browser cache.

### Interaction Targets
| Target | Metric | Technology |
|---|---|---|
| **First Contentful Paint** | < 800ms | Vite Compression + Self-hosted Fonts. |
| **Interaction to Response** | < 100ms | Optimistic UI + Vue Reactive Engine. |
| **Package Size** | < 250kb (Gzip) | Mandatory Code Splitting + Tree Shaking. |

---

## 🧬 05. Industrial Skeleton Loading Pattern

Content must appear to be "already there" before the data arrives.

1.  **Structure-First**: Skeletons must mirror the final grid/row layout exactly.
2.  **Shimmer Logic**: CSS-only animation using `--surface-2` to `--surface-3`.
3.  **Automatic Skeletons**: Use Vue's `Suspense` and `v-if` to toggle between `<DataTableSkeleton />` and `<DataTable />`.

---

## 🛡️ 06. Deterministic CSS Mapping

We use Tailwind CSS for utility-first styling with strict industrial constraints.

- **Deterministic Logic**: Use `clsx` and `tailwind-merge` to prevent class-clobbering and ensure predictable overrides.
- **Root Mapping**: Global state (like `isDark`) is mapped directly to `document.documentElement.classList` for zero-flash theme persistence.
- **Token Compliance**: All colors must use design tokens (`bg-void`, `text-amber`) instead of arbitrary hex codes.

---

## ✅ 07. Implementation Roadmap

1.  **[x] Phase 1 — Token Foundation**: CSS custom properties and IBM Plex self-hosting.
2.  **[x] Phase 2 — The 4-Side Shell**: Persistent persistent HTML/CSS frames.
3.  **[/] Phase 3 — Component Standard**: Building the **No-Spinner** Vue component library.
4.  **[ ] Phase 4 — Bandwidth Layer**: Implementing ETag/304 validation on the Go backplane.
5.  **[ ] Phase 5 — Performance Audit**: 60fps scrolling and < 100ms interaction verification.

---
*Last Updated: April 2026 | Following the Universal SaaS Engineering Standard.*
