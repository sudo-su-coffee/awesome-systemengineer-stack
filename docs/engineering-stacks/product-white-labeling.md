# 🎨 Universal SaaS: Product White-Labeling Engineering Stack

This document defines the **Business Soul** and **UX Sovereignty** of the platform—synthesizing **Industrial Theme Orchestration**, **Universal Globalism (i18n/l10n)**, and **Inclusive Accessibility (A11Y)**. It is the blueprint for a white-labeled, global-scale product interface.

---

## 🏛️ 01. Theme Orchestration (The Atomic Brand)

We move beyond hard-coded colors into a dynamic, tenant-aware design system.

### 1. CSS Variable Architecture (Reactive Branding)
Every visual element in the 4-side shell interface is mapped to a set of reactive CSS Custom Properties.
- **Surface Variables**: `--bg-primary`, `--bg-secondary`, `--surface-border`.
- **Brand Variables**: `--brand-primary`, `--brand-secondary`, `--brand-accent`.
- **Typography Variables**: `--font-family-primary`, `--text-main-size`.

### 2. Dynamic Injection Lifecycle
When a tenant accesses the platform (e.g., via `client-a.app.com`):
1.  **Handshake**: The Frontend fetches the **Branding Meta** from the Core App (T3).
2.  **Injection**: The browser injects a dynamic `<style id="tenant-theme">` block into the head, overriding the default variable values.
3.  **Mutation**: Visual components (Vue 3) automatically reflect the new branding without a reload.

---

## 🌍 02. Universal Globalism (i18n & l10n)

To be a "Universal SaaS," the platform must speak every language and understand every currency.

- **i18n (Internationalization)**: Use of **Vue-I18n** with lazy-loaded language chunks. Support for **RTL (Right-to-Left)** languages like Arabic/Hebrew via logical CSS properties (e.g., `margin-inline-start`).
- **l10n (Localization)**: 
    - **Currency**: Automated formatting based on the tenant's locale (e.g., ₹ vs $ vs €).
    - **Timezones**: Every timestamp is stored as UTC in the DB and localized on the client-side using **Intl.DateTimeFormat** or **Day.js**.

---

## ♿ 03. Inclusive Accessibility (A11Y)

We follow the **Zoho-Standard** of extreme accessibility for industrial use cases.

- **WCAG 2.1 Compliance**: Mandatory adherence to **Level AA** standards. 
- **Semantic Foundation**: Use of native HTML5 elements (`<button>`, `<main>`, `<nav>`) to ensure perfect screen-reader compatibility.
- **Focus Mastery**: Every interactive element must have a clear, high-contrast `:focus` state.
- **Color Contrast**: Automated CI-check to ensure brand-primary colors meet the 4.5:1 contrast ratio against background surfaces.

---

## 🏷️ 04. Whitelabel Assets & Domain Mapping

Multi-tenancy at the domain and identity level.

- **Custom Domain Ingress**: Support for CNAME mapping (e.g., `portal.client-a.com` -> `ingress.whatomate.app`). Certificates are automatically provisioned via **Let's Encrypt / Cert-Manager**.
- **The "Sovereign Logo" Hub**: Every tenant provides their own Favicon, App-Icon, and Logo-SVG. These are served from the **Media Hub (T10 - MinIO)** but cached at the **Edge (T1 - Nginx)**.

---

## 🧪 05. The "Zero-Clash" Prototype Kit

We maintain a "Vanilla" foundation that never conflicts with tenant branding.

- **Generic Utility Classes**: Use of utility-first CSS *within* the design system tokens to ensure layout stability regardless of color-swapping.
- **Dark-Mode-by-Default**: Every theme must have a "High-Contrast Dark" variation for industrial warehouse/night-shift environments.

---
*Last Updated: April 2026 | Following the Universal SaaS Engineering Standard.*
