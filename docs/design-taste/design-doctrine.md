---
name: Artisanal Logic
colors:
  surface: '#fcf9f8'
  surface-dim: '#dcd9d9'
  surface-bright: '#fcf9f8'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f6f3f2'
  surface-container: '#f0eded'
  surface-container-high: '#eae7e7'
  surface-container-highest: '#e5e2e1'
  on-surface: '#1b1c1c'
  on-surface-variant: '#4e4449'
  inverse-surface: '#303030'
  inverse-on-surface: '#f3f0ef'
  outline: '#807479'
  outline-variant: '#d1c3c9'
  surface-tint: '#76546a'
  primary: '#32172a'
  on-primary: '#ffffff'
  primary-container: '#4a2c40'
  on-primary-container: '#bb93ab'
  inverse-primary: '#e5bad4'
  secondary: '#75584d'
  on-secondary: '#ffffff'
  secondary-container: '#fed7ca'
  on-secondary-container: '#795c51'
  tertiary: '#212120'
  on-tertiary: '#ffffff'
  tertiary-container: '#363635'
  on-tertiary-container: '#a09e9d'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#ffd8ee'
  primary-fixed-dim: '#e5bad4'
  on-primary-fixed: '#2d1225'
  on-primary-fixed-variant: '#5d3d52'
  secondary-fixed: '#ffdbce'
  secondary-fixed-dim: '#e4beb2'
  on-secondary-fixed: '#2b160f'
  on-secondary-fixed-variant: '#5b4137'
  tertiary-fixed: '#e5e2e0'
  tertiary-fixed-dim: '#c8c6c4'
  on-tertiary-fixed: '#1b1c1b'
  on-tertiary-fixed-variant: '#474745'
  background: '#fcf9f8'
  on-background: '#1b1c1c'
  surface-variant: '#e5e2e1'
typography:
  display-lg:
    fontFamily: Montserrat
    fontSize: 48px
    fontWeight: '700'
    lineHeight: 56px
    letterSpacing: -0.02em
  headline-lg:
    fontFamily: Montserrat
    fontSize: 32px
    fontWeight: '600'
    lineHeight: 40px
    letterSpacing: -0.01em
  headline-md:
    fontFamily: Montserrat
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
  headline-sm:
    fontFamily: Montserrat
    fontSize: 20px
    fontWeight: '600'
    lineHeight: 28px
  body-lg:
    fontFamily: Montserrat
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
  body-md:
    fontFamily: Montserrat
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 20px
  body-sm:
    fontFamily: Montserrat
    fontSize: 13px
    fontWeight: '400'
    lineHeight: 18px
  label-md:
    fontFamily: Montserrat
    fontSize: 12px
    fontWeight: '600'
    lineHeight: 16px
    letterSpacing: 0.05em
  label-sm:
    fontFamily: Montserrat
    fontSize: 11px
    fontWeight: '500'
    lineHeight: 14px
  headline-lg-mobile:
    fontFamily: Montserrat
    fontSize: 28px
    fontWeight: '600'
    lineHeight: 36px
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  unit: 4px
  container-padding: 24px
  gutter: 16px
  widget-gap: 20px
  stack-sm: 8px
  stack-md: 16px
  stack-lg: 32px
---

## Brand & Style
This design system bridges the gap between high-end artisanal craftsmanship and high-performance data management. The brand personality is authoritative yet grounded, evoking the feeling of a luxury boutique's back-office operations: efficient, precise, and sophisticated. 

The design style follows a **Corporate Modern** approach with a **Minimalist** execution. It prioritizes information density and clarity through clean white surfaces, systematic border usage, and generous whitespace. The aesthetic avoids unnecessary decoration, instead finding beauty in the rhythm of well-structured data and the warmth of a refined, heritage-inspired color palette.

## Colors
The palette is anchored by a deep **Aubergine Primary**, providing a sense of premium stability. A **Warm Taupe** serves as the secondary accent, nodding to artisanal materials like leather and earth.

The interface primarily utilizes a "Paper and Ink" philosophy:
- **Surfaces:** Clean, high-brightness whites (#FFFFFF) for the primary background and light warm grays (#FBF9F8) for secondary containers.
- **Borders:** A systematic use of $tertiary_color_hex at 100% opacity for structural definition and 50% for subtle dividers.
- **Semantics:** Status indicators are slightly desaturated to maintain the professional tone while ensuring clear functional communication. Success is represented by a mossy green, warning by a muted ochre, and error by a terracotta red.

## Typography
Montserrat is used exclusively to maintain a bold, geometric, and modern presence. 

- **Headlines:** Use Bold (700) or SemiBold (600) weights. Larger display titles should use slight negative letter spacing to feel "tighter" and more editorial.
- **Body:** Standardized on 14px (body-md) for data density. Use the 400 weight for optimal legibility against white backgrounds.
- **Labels:** Small labels use uppercase styling with increased letter spacing to provide a clear distinction from interactive text elements.
- **Hierarchy:** Maintain a clear vertical rhythm. Use `headline-sm` for widget titles and `label-md` for table headers.

## Layout & Spacing
The design system employs a **12-column fluid grid** for the main content area, housed within a fixed-width sidebar navigation model.

- **Grid:** On desktop, use a 24px outer margin and 16px gutters between columns.
- **Sidebar:** Fixed at 260px for standard navigation, collapsible to 64px for expanded workspace.
- **Widgets:** Dashboard widgets should span column increments (e.g., 3, 4, 6, or 12).
- **Rhythm:** Spacing follows a 4px baseline. Use 16px (stack-md) for standard vertical spacing between elements within a card and 8px (stack-sm) for grouping related inputs or labels.

## Elevation & Depth
This design system avoids heavy shadows in favor of **Low-Contrast Outlines** and **Tonal Layers**.

- **Level 0 (Background):** The canvas uses a very subtle warm-white tint (#FBF9F8).
- **Level 1 (Cards/Widgets):** Surfaces are pure white (#FFFFFF) with a 1px solid border (#F5F2F0). A very soft, ambient shadow (0px 2px 4px rgba(74, 44, 64, 0.04)) may be used to provide a "lift" from the background.
- **Level 2 (Overlays/Modals):** Pure white surfaces with a more pronounced shadow (0px 10px 20px rgba(0, 0, 0, 0.08)) and a 1px border.
- **Interactive States:** On hover, clickable elements like cards may transition to a slightly darker border color (#8D6E63 at 20% opacity).

## Shapes
A **Soft** shape language is used to maintain professional rigor while remaining approachable.

- **Small Components:** Buttons, input fields, and tags use a 4px (0.25rem) radius.
- **Medium Components:** Dashboard cards and modal containers use an 8px (0.5rem) radius.
- **Interactive Indicators:** Checkboxes use a 2px radius; radio buttons remain fully circular.
- **Strictness:** Do not use fully rounded "pill" shapes for primary actions; reserve rounded-full only for status badges and user avatars.

## Components
- **Data Tables:** Use a "Zephyr" style with alternating row tints (#FBF9F8) and 1px horizontal dividers. Table headers should be sticky with a solid aubergine bottom border (2px).
- **Buttons:** 
    - *Primary:* Solid Aubergine with White text.
    - *Secondary:* Outlined Taupe with Taupe text.
    - *Ghost:* No border, Aubergine text for tertiary actions.
- **Inputs:** Use "Floating Label" or "Top-Aligned Label" styles. Borders should be 1px solid gray-light, turning Aubergine on focus.
- **Dashboard Widgets:** Every widget must have a consistent header containing a `headline-sm` title and an optional right-aligned action area for filters or time-range pickers.
- **Status Chips:** Small, condensed labels with a light background tint (10% opacity) of the semantic color and a dark solid text of the same hue.
- **Checkboxes & Radios:** Use the primary color for selected states. Ensure the hit area is a minimum of 40x40px for accessibility, even if the visual mark is smaller.