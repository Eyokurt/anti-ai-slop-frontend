---
name: anti-ai-slop-frontend
description: Production-grade frontend and UI craftsmanship guidelines that eliminate generic AI slop. Enforces clean aesthetics, strict semantic HTML, complete 4-state components, SVG-only iconography with an absolute ban on Unicode emojis, authentic mock data, and modern CSS/Tailwind discipline. MUST be loaded and followed whenever planning, creating, refactoring, or reviewing frontend code, UI components, styling, or pages.
---

# Anti-AI-Slop Frontend Craftsmanship

## Overview
AI coding assistants naturally gravitate toward predictable, generic, and uninspired frontend code: neon purple gradients on dark backgrounds, repetitive 3-column card layouts, Unicode emoji spam in buttons/badges, broken placeholder image paths, and raw `div` tags with `onClick` handlers.

This skill establishes uncompromising engineering and design standards to produce human-crafted, production-ready, and resilient frontend interfaces.

---

## 1. Absolute Emoji Prohibition & Strict Iconography

### The Zero-Emoji Rule
- **NEVER use Unicode emojis anywhere in the UI** (e.g., 🚀, ✨, 🔥, ⚡, 🎉, 💡, ❌, ✅, 👉, 🤖).
- **Prohibited Placements**:
  - Buttons: `<button>🚀 Get Started</button>` is strictly forbidden.
  - Badges / Status Pills: `<span>✨ New</span>`, `<span>🔥 Trending</span>` are forbidden.
  - Headings & Labels: `<h2>⚡ Features</h2>` is forbidden.
  - Tables, toasts, navigation links, and empty states.
- **Why**: Emojis render inconsistently across operating systems, cannot adapt to CSS theme shifts or `currentColor`, break typographic hierarchy, and instantly brand an interface as low-effort AI slop.

### Vector SVG Icon Standards
- Use dedicated, monochromatic vector SVG icon packages (e.g., Lucide React, Heroicons, Phosphor Icons, Radix Icons, or Tabler).
- **Inherit Color**: Icons must inherit color via `currentColor` so they harmonize naturally with text and theme states.
- **Optical Sizing**: Match icon sizes to adjacent text line-height:
  - `text-xs` / `text-sm` → `size-3.5` (14px) or `size-4` (16px)
  - `text-base` → `size-4` (16px) or `size-5` (20px)
  - `text-lg` / `text-xl` → `size-5` (20px) or `size-6` (24px)
- **Accessibility**:
  - Decorative icons adjacent to text MUST have `aria-hidden="true"`.
  - Standalone icon buttons MUST have an explicit `aria-label` (e.g., `<button aria-label="Close modal"><X className="size-4" aria-hidden="true" /></button>`).

---

## 2. Placeholder & Mock Asset Hygiene

### Ban on Broken Local Asset Paths
- **NEVER hallucinate non-existent local files** (e.g., `<img src="/placeholder.svg" />`, `<img src="/images/hero.png" />`, `src="/avatar.jpg"`).
- If an asset does not exist yet:
  - Use inline clean SVG illustrations.
  - Use deterministic seeded image services: `https://picsum.photos/seed/{unique_slug}/800/600`.
  - Use CSS-based subtle geometric or textured background patterns.

### Avatar Fallback Discipline
- Never render raw broken `<img>` tags for user avatars.
- Always implement an **Initials Fallback** pattern:
  ```tsx
  // Recommended Pattern:
  <div className="flex size-8 items-center justify-center rounded-full bg-neutral-200 text-xs font-medium text-neutral-700 dark:bg-neutral-800 dark:text-neutral-300">
    EK
  </div>
  ```
- In lists or tables, every user must have unique initials or distinct seeded visuals. Never replicate the exact same avatar across multiple rows.

### Input Placeholders are NOT Labels
- Never omit `<label>` in favor of placeholder text.
- Never duplicate the label inside the placeholder (e.g., `Label: "Email"`, `Placeholder: "Enter your email"` is lazy duplication).
- Use placeholders **strictly** for expected syntax, format hints, or real examples:
  - Email: `alex@company.com`
  - Phone: `+90 (555) 012-3456`
  - Domain: `app.yourbrand.com`
- Search inputs must never say `"Search here..."` or `"Type something..."`. Use domain-specific prompts: `"Filter transactions by ID or customer..."`.

### Zero Tolerance for Lorem Ipsum
- Strictly prohibit `Lorem ipsum dolor sit amet...`, `"Sample description"`, `"Feature 1, Feature 2"`, and `"Title goes here"`.
- Always author realistic, contextual copy fitting the user's business domain (e.g., for analytics: *"PostgreSQL connection pool exhausted at 14:02 UTC"*, not *"Error occurred"*).

### Realistic Data Variance
- In mock tables and list feeds, vary the data realistically:
  - **Statuses**: Blend multiple states (`Active`, `Pending`, `Failed`, `Refunded`).
  - **Timestamps**: Use organic relative times (`4 mins ago`, `Yesterday, 18:22`, `Oct 14`).
  - **Numbers**: Use non-round numbers (`$142.80`, `47.2%`, `1,289 req/s` instead of `$100` and `99.9%`).

---

## 3. Aesthetic & Visual Philosophy (Kill the AI Archetype)

### Color & Surfaces
- **No Cliche AI Neon Gradients**: Do NOT default to dark mode surfaces featuring saturated neon purple/violet/cyan glowing drop-shadows or borders (`box-shadow: 0 0 25px rgba(139, 92, 246, 0.5)`).
- **Grounded Base Palette**: Use off-blacks (`#0a0a0a`, `#121212`, `#0f172a`), high-contrast neutrals, and at most **ONE** intentional brand accent color.
- **Subtle Surfaces Over Thick Borders**: Separate elevation and sections using subtle contrast steps (`bg-neutral-900` over `bg-neutral-950`, or `bg-neutral-50` inside `bg-white`) rather than harsh 1px borders everywhere.

### Layout & Composition
- **Break Card Uniformity**: Avoid the standard "3 equal cards in a row with an icon in a square". Explore:
  - Asymmetric 2-column hero layouts.
  - Staggered masonry or Bento-style grids with varying cell weights.
  - Dense, informative list views with micro-metadata instead of oversized empty cards.
- **Viewport Stability**: Always use `min-h-dvh` instead of `100vh` or `h-screen` to eliminate layout bouncing on mobile browser address bar shifts.

### Typography
- Avoid default browser Inter for everything without deliberate styling.
- Pair modern geometric or humanist typefaces (Geist, Outfit, Satoshi, Plus Jakarta Sans) or clean system font stacks.
- Use `text-wrap: balance` on headlines and `text-wrap: pretty` on paragraphs to eliminate orphaned single words.
- Enable `tabular-nums` (`font-variant-numeric: tabular-nums`) on all numeric tables and metric counters.

---

## 4. Semantic HTML & Accessibility (Strict Div-Soup Ban)

- **Zero Div-Buttons**: Never write `<div onClick={...}>`. Always use semantic `<button type="button">` or `<a href="...">`.
- **Semantic Structure**: Use `<nav>`, `<header>`, `<main>`, `<aside>`, `<section>`, `<article>`, `<figure>`, and `<footer>`.
- **Keyboard Navigation**:
  - Interactive elements must have prominent, visible focus states:
    `focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-primary`
  - Never remove focus rings without an accessible replacement.
- **Form Controls**:
  - Always link `<label htmlFor="id">` to its respective input.
  - Group related fields with `<fieldset>` and `<legend>`.
  - Associate helper or error messages with `aria-describedby`.

---

## 5. Mandatory 4-State Component Architecture

For any data-driven or interactive component, you are strictly forbidden from implementing only the "happy path". Every component must handle:

1. **Loading State**:
   - Use shape-matching skeleton loaders (`animate-pulse` blocks mimicking the actual text lines and avatars).
   - Do NOT drop a solitary spinner in the center of an empty viewport.
2. **Empty State**:
   - Composed, friendly zero-data UI explaining what belongs here.
   - Include a primary action CTA button (e.g., "Create your first project").
3. **Error State**:
   - Clear inline recovery UI with an actionable retry button (e.g., "Failed to load invoices. [Try Again]").
   - Never show silent failures or fallback to native `alert()`.
4. **Interactive States**:
   - Every clickable control must have distinct `:hover`, `:focus-visible`, `:active` (`active:scale-[0.98]`), and `:disabled` (`disabled:opacity-50 disabled:pointer-events-none`) visual feedback.

---

## 6. CSS & Tailwind Disipline

- **No Class Bloat**: Do not write 80+ character utility strings inline when subcomponents or composition patterns should be used.
- **Strict Theme Tokens**: Never use arbitrary pixel classes (e.g., `w-[347px]`, `text-[#637281]`, `rounded-[13px]`). Always stick to design system tokens (`w-80`, `text-muted-foreground`, `rounded-xl`).
- **Clean Z-Index Hierarchy**: Never spray arbitrary values like `z-[9999]`. Use a standardized scale (`z-10`, `z-20`, `z-50`).

---

## 7. Motion & Animation Restraint

- **No Gratuitous Framer Motion**: Do NOT wrap every heading, card, and paragraph in `initial={{ opacity: 0, y: 20 }}` entrance animations.
- **Hardware Acceleration**: Only animate CSS `transform` and `opacity`. Never animate layout triggers (`width`, `height`, `top`, `margin`).
- **Snappy Micro-interactions**: Keep transitions fast and purposeful (150ms to 250ms duration with `ease-out`).

---

## 8. Copywriting Authenticity (Anti-Buzzword Policy)

- **Banned AI Buzzwords**: NEVER write:
  - *"Unlock the power of..."*
  - *"Elevate your experience"*
  - *"Seamless integration"*
  - *"Unleash your potential"*
  - *"Supercharge your workflow"*
  - *"Next-gen solution"*
  - *"Delve into..."*
  - *"Game-changer"*
- **Human Tone**: Write clear, active, concise copy that describes concrete capabilities:
  - *Bad*: "Supercharge your database performance with our next-gen engine."
  - *Good*: "Query across 10 million rows in under 12 milliseconds."

---

## Pre-Flight Verification Checklist

Before presenting frontend code or declaring a UI task complete, audit against this checklist:

- [ ] **No Emojis**: Are all buttons, badges, headers, and toasts 100% free of Unicode emojis?
- [ ] **No Broken Images**: Are all `<img>` tags pointing to real files, seeded placeholders, or initial fallbacks?
- [ ] **No Div Buttons**: Are all clickable controls actual `<button>` or `<a>` tags?
- [ ] **Focus Visible**: Can the entire component be navigated and operated using only the `Tab` and `Enter`/`Space` keys?
- [ ] **4 States Handled**: Did you provide Loading (Skeleton), Empty, Error, and Active states?
- [ ] **No Generic Buzzwords**: Is the text clear, professional, and free of AI marketing clichés?
- [ ] **No Arbitrary CSS Hacks**: Are styles anchored to design tokens rather than random bracketed pixel values?
