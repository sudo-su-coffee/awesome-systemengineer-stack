---
name: sovereign-master-taste
description: The ultimate High-Agency Design & Engineering Doctrine. Consolidates the entire Taste-Skill framework into a single source of truth. Enforces premium aesthetics, mechanical sympathy, and zero-truncation output for AI agents.
---

# 🕋 SOVEREIGN MASTER TASTE (ULTIMATE DOCTRINE)

## 1. ACTIVE BASELINE CONFIGURATION
* **DESIGN_VARIANCE:** 8 (1=Symmetry, 10=Artsy Chaos)
* **MOTION_INTENSITY:** 7 (1=Static, 10=Cinematic Physics)
* **VISUAL_DENSITY:** 4 (1=Art Gallery, 10=Pilot Cockpit)
* **PREFERRED_VIBE:** "AUTO" (Options: AUTO, SOFT, BRUTALIST, MINIMALIST)

**Instruction:** Adapt these values dynamically if the user requests it. Use them to drive the logic in all sections below.

---

## 2. THE UNIVERSAL BAN LIST (ANTI-SLOP)
If your output includes ANY of these, it is a critical failure. Self-correct immediately.

### A. Visual & CSS
* **NO "AI Purple/Blue" Glows:** Avoid the generic neon gradient aesthetic. No purple button glows.
* **NO Inter Font:** Strictly BANNED for premium contexts. Use `Geist`, `Satoshi`, `Outfit`, or `Clash Display`.
* **NO Pure Black:** Never use `#000000`. Use Off-Black, Zinc-950, or Charcoal (`#0A0A0A`).
* **NO `h-screen`:** BANNED for Hero sections. ALWAYS use `min-h-[100dvh]` to prevent mobile layout jumping.
* **NO Generic Shadows:** Standard `shadow-md` is too harsh. Use custom diffused, tinted shadows.
* **NO Emojis:** Strictly BANNED in code, markup, and text. Use high-quality icons or SVG primitives.

### B. Content & Copywriting
* **NO Startup Slop:** Never use: "Elevate", "Seamless", "Unleash", "Next-Gen", "Game-changer", "Delve", "Tapestry".
* **NO Generic Names:** Banned: "John Doe", "Acme Corp", "Nexus", "SmartFlow".
* **NO Fake Numbers:** Avoid `99.99%`, `50%`, or `$100.00`. Use organic data: `47.2%`, `$94.00`.
* **NO Lorem Ipsum:** Never use placeholder Latin. Write contextually relevant draft copy.

---

## 3. THE VIBE ENGINE (ARCHETYPES)
Select ONE archetype based on `PREFERRED_VIBE` or the prompt context. Do not mix paradigms.

### 🏺 1. SOFT LUXURY (Awwwards-tier / SaaS / Apple-esque)
* **Palette:** Deepest OLED blacks or pure whites. Max 1 desaturated accent (Emerald, Deep Rose).
* **Radii:** Exaggerated squircles/rounded corners (`rounded-[2.5rem]`).
* **Haptics:** "Double-Bezel" (Doppelrand) card architecture — a shell wrapper with an inner core.
* **Lighting:** Diffused ambient shadows tinted to background hue.

### 🏭 2. INDUSTRIAL BRUTALIST (Blueprints / Retro-Tech / Military)
* **Palette:** "Swiss Print" (Newsprint/Off-white + Carbon Ink + Hazard Red) or "Tactical Telemetry" (Dark + Phosphor White).
* **Radii:** Absolute zero. Exactly 90-degree corners.
* **Geometry:** Visible 1px structural grids. Elements anchored to intersections.
* **Type:** Massive uppercase display headers with negative tracking (`-0.05em`) + Monospaced data blocks.

### 📖 3. UTILITARIAN MINIMALIST (Editorial / Linear / Notion-style)
* **Palette:** Warm monochrome (`#FBFBFA`) with desaturated spot pastels (Pale Red: `#FDEBEC`).
* **Grid:** Flat Bento Grids with exactly `1px solid #EAEAEA` borders.
* **Architecture:** Crisp 8px-12px radii. No gradients. No heavy shadows.
* **Depth:** Use desaturated images with 4% warm grain overlays.

---

## 4. COMPONENT MASTER-CLASS

### 4.1 "Double-Bezel" Card Architecture
Never place a container flat. All primary cards must use nested enclosures:
*   **Outer Shell:** `p-1.5 rounded-[2.5rem] bg-white/5 ring-1 ring-white/10`.
*   **Inner Core:** `rounded-[calc(2.5rem-0.375rem)] bg-zinc-950 shadow-[inset_0_1px_1px_rgba(255,255,255,0.1)]`.

### 4.2 "Button-in-Button" CTA
Primary interactive buttons must be fully rounded pills (`rounded-full`):
*   **Structure:** If a button has an icon (`↗`), nest it inside its own circular wrapper (`w-8 h-8 rounded-full bg-white/10`) flush with the button's right inner padding.
*   **Physics:** On `:active`, use `-translate-y-[1px]` or `scale-[0.98]` for tactile success.

---

## 5. MOTION & PERFORMANCE (MECHANICAL SYMPATHY)

### 5.1 The Motion Engine
* **Physics:** Strictly use Spring Physics (`stiffness: 100, damping: 20`). No linear easing.
* **Magnetic Hover:** Buttons should physically "pull" toward the cursor.
    *   **CRITICAL:** Use Framer Motion's `useMotionValue` outside the React render cycle to prevent performance collapse.
* **Staggered Orchestration:** Elements never mount instantly. Use `translate-y-12 blur-md opacity-0` resolvers with 100ms staggered delays.

### 5.2 Performance Guardrails
* **Animation Property Lock:** Animate EXCLUSIVELY via `transform` and `opacity`. NEVER animate `top`, `left`, `width`, or `height`.
* **GPU Preservation:** Apply grain/noise filters exclusively to fixed, `pointer-events-none` pseudo-elements. Never apply blur to scrolling containers.
* **RSC Management:** Wrap interactive motion components in `'use client'` leaf components. Keep the main layout as Server Components.

---

## 6. REDESIGN & AUDIT PROTOCOL
When improving existing code, scan for these "AI Tells" and fix them:
1.  **Orphaned Words:** Use `text-wrap: balance` on all headers.
2.  **Wide Body Text:** Limit paragraphs to `max-w-[65ch]`.
3.  **Proportional Data:** Ensure all data/numbers use `font-variant-numeric: tabular-nums` or `font-mono`.
4.  **Uniform Borders:** Vary radii — softer on containers, tighter on inner elements.
5.  **Empty States:** Never show a blank dashboard. Design a composed "get started" view.

---

## 7. FULL OUTPUT ENFORCEMENT (ANTI-LAZINESS)
* **The Token Mandate:** Every task is production-critical. Truncation is a failure.
* **Banned Patterns:** `// ...`, `// rest of code`, `// implement here`, `/* ... */`.
* **Breakpoint Pause:** If the output is too long, pause at a clean code boundary and end with:
  `[PAUSED — X of Y complete. Send "continue" to resume from: next section name]`
* **No Shortcuts:** Never describe what code *would* do. Write the actual un-skipped implementation.

---
*Sovereign Master Taste | BlackLoverTech Industrial Standard | April 2026*
*Created by Antigravity | Mechanical Sympathy — Always Sovereign, Never Locked*
