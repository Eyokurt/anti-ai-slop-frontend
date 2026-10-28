---
name: anti-ai-slop-frontend
description: Rules against generic AI-looking frontend: no emojis in UI chrome, complete async states (loading/empty/error/success), interactive pseudo-states, semantic HTML, realistic mock data, restrained editorial layouts. Use when building or reviewing UI code.
---

# Anti-AI-Slop Frontend Craftsmanship

## Overview
AI coding assistants naturally gravitate toward predictable, generic frontend archetypes: dark purple gradients, repetitive 3-column card grids, emoji spam in buttons and badges, hallucinated placeholder paths, and raw `div` tags with `onClick` handlers. When pushed away from these patterns, models often over-compensate by building hyper-dense sci-fi dashboards or academic theses.

This skill establishes balanced design and engineering standards to produce human-crafted, accessible, and resilient frontend interfaces.

---

## 1. Aesthetic Intent First (Avoid Default Archetypes)

Before writing markup or CSS, determine an intentional aesthetic direction tailored to the specific product and target audience:
- **Clean Grotesque / High-Density Utility**: Tight data tables, crisp monochromatic borders, neutral surfaces, functional monospace accents (e.g., developer tools, financial consoles).
- **Warm Humanist Editorial**: High-contrast serif headlines, warm stone/linen backgrounds, generous whitespace, relaxed leading (e.g., publications, lifestyle, bespoke services).
- **Industrial Precision**: Deep neutral surfaces, technical line dividers, stark typography, restrained functional accents (e.g., hardware, manufacturing, logistics).
- **Vibrant Neo-Clean**: Soft pastel accents on pure white/neutral canvases, rounded geometry, punchy high-contrast typography (e.g., consumer fintech, modern SaaS).

*Never default blindly to dark mode with purple/indigo glows or beige surfaces with generic serif headings. Choose a distinct point of view based on who uses the product.*

---

## 2. Iconography & Emoji Boundaries

### The UI Chrome Zero-Emoji Rule
- **No Unicode emojis in UI controls**: Never place emojis (🚀, ✨, 🔥, ⚡, 🎉, 💡, ❌, ✅, 👉) inside buttons, status badges, section headings, table headers, toasts, or navigation links.
  - *Bad*: `<button>🚀 Get Started</button>`, `<span class="badge">✨ New</span>`
  - *Good*: `<button>Get Started</button>`, `<span class="badge">New</span>` (with an optional vector icon)
- **User-Generated Content (UGC) Exemption**: Emojis are fully permissible inside actual user-submitted content (e.g., chat messages, customer review quotes, comment threads). The ban applies exclusively to application UI chrome.
- **Why**: Emojis render inconsistently across operating systems, cannot adapt to theme changes or `currentColor`, break typographic hierarchy, and immediately brand an interface as low-effort prototype copy.

### Vector SVG Icon Standards
- Use dedicated, monochromatic vector SVG icon libraries (e.g., Lucide, Heroicons, Phosphor, Radix Icons).
- **Inherit Color**: Icons must inherit color via `currentColor` so they harmonize naturally with text and theme states.
- **Optical Sizing**: Match icon dimensions to adjacent text line-height:
  - `text-xs` / `text-sm` → `size-3.5` (14px) or `size-4` (16px)
  - `text-base` → `size-4` (16px) or `size-5` (20px)
  - `text-lg` / `text-xl` → `size-5` (20px) or `size-6` (24px)
  *(Note: `size-*` requires Tailwind v3.4+; use `w-4 h-4` for older versions).*
- **Accessibility**:
  - Decorative icons adjacent to text MUST include `aria-hidden="true"`.
  - Standalone icon buttons MUST include an explicit `aria-label` (e.g., `<button aria-label="Close dialog"><X className="size-4" aria-hidden="true" /></button>`).

---

## 3. Placeholder & Mock Asset Hygiene

### Local Asset Hallucination Ban
- **Never reference non-existent local file paths** (e.g., `<img src="/placeholder.svg" />`, `<img src="/images/hero.png" />`, `src="/avatar.jpg"`).
- If an asset is not yet available:
  - Use inline clean SVG illustrations or geometric background patterns.
  - Avoid unvetted external placeholder domains (`picsum.photos`, etc.) which fail under strict CSPs, offline sandboxes, or ad-blockers.
  - For user avatars, always implement an **Initials Fallback**:
    ```tsx
    <div className="flex size-8 items-center justify-center rounded-full bg-neutral-200 text-xs font-medium text-neutral-700 dark:bg-neutral-800 dark:text-neutral-300">
      EK
    </div>
    ```

### Input Placeholders are NOT Labels
- Never omit `<label>` in favor of placeholder text.
- Never duplicate the label inside the placeholder (e.g., `Label: "Email"`, `Placeholder: "Enter your email"` is redundant).
- Use placeholders strictly for syntax hints or authentic format examples:
  - Email: `alex@company.com`
  - Domain: `app.yourdomain.com`
  - Search: Use domain-specific prompts (`"Filter by account ID or name..."` instead of `"Search here..."`).

### Zero Tolerance for Lorem Ipsum
- Prohibit `Lorem ipsum...`, `"Sample description"`, `"Feature 1, Feature 2"`, and `"Title goes here"`.
- Always write authentic, domain-appropriate copy fitting the product's business context.

### Realistic Data Variance (Dashboards & Lists)
- In operational tools, data tables, and feeds, introduce organic variation:
  - **Statuses**: Blend multiple realistic states (`Active`, `Pending`, `Failed`, `Refunded`).
  - **Timestamps**: Use relative times (`4 mins ago`, `Yesterday, 18:22`, `Oct 14`).
  - **Numbers**: Use non-round numbers (`$142.80`, `47.2%`, `1,289 req/s` instead of `$100` and `99.9%`).

---

## 4. Visual Philosophy & Surface Hierarchy

### Color, Contrast & Surfaces
- **No Cliché Neon Gradients**: Avoid default dark mode surfaces dominated by saturated purple/violet/cyan glowing drop-shadows or borders (`box-shadow: 0 0 25px rgba(139, 92, 246, 0.5)`).
- **Grounded Base Palette**: Use natural off-blacks (`#0a0a0a`, `#121212`, `#0f172a`), balanced neutrals, and at most **one** intentional brand accent color.
- **Subtle Surface Steps**: Separate elevation through subtle contrast steps (`bg-neutral-900` over `bg-neutral-950`, or `bg-neutral-50` inside `bg-white`) rather than harsh borders around every element.
- **WCAG Contrast Compliance**: Ensure text meets WCAG 2.1 AA standards (minimum 4.5:1 contrast ratio for regular body text; 3:1 for large headings and UI boundary states). Never pair low-contrast muted gray text with faint dark surfaces.

### Layout & Composition
- **Break Card Uniformity**: Avoid the standard "3 equal cards in a row with an icon in a square". Explore:
  - Asymmetric hero layouts (one dominant focus block paired with concise supporting content).
  - Staggered masonry or Bento-style grids with varying cell weights.
  - Dense, informative list views with micro-metadata instead of oversized empty cards.
- **Viewport Stability**: Prefer `min-h-dvh` over `100vh` or `h-screen` to eliminate layout bouncing caused by mobile browser address bars. *(Note: requires Tailwind v3.4+ or custom CSS `min-height: 100dvh`).*

### Typography Discipline
- Avoid defaulting to generic sans-serif fonts without brand alignment. Pair typography deliberately with the product archetype.
- Avoid treating trending fonts (e.g. Geist, Outfit) as automatic silver bullets; clean system font stacks (`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`) are often cleaner and perform better.
- Use `text-wrap: balance` on headlines and `text-wrap: pretty` on body copy to eliminate orphaned words.
- Use `tabular-nums` (`font-variant-numeric: tabular-nums`) on all numeric tables, metrics, and currency displays.

---

## 5. Semantic HTML, Accessibility & Interactive States

- **Zero Div-Buttons**: Never write `<div onClick={...}>`. Always use semantic `<button type="button">` or `<a href="...">`.
- **Semantic Page Landmarks**: Use `<nav>`, `<header>`, `<main>`, `<aside>`, `<section>`, `<article>`, `<figure>`, and `<footer>`.
- **Interactive Control States**: Every interactive control (`<button>`, `<a>`, `<input>`, `<select>`) must handle all interaction states:
  - Default / Idle
  - `:hover`
  - `:focus-visible` (visible ring indicator: `focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-primary`, never suppressed)
  - `:active` (tactile micro-feedback, e.g. `active:scale-95`)
  - `:disabled` (`disabled:opacity-50 disabled:pointer-events-none`)
- **Form Controls**:
  - Always link `<label htmlFor="id">` to its input.
  - Group related inputs using `<fieldset>` and `<legend>`.
  - Associate validation errors and hints via `aria-describedby`.

---

## 6. Async Lifecycle States (Loading / Empty / Error / Success)

For any component that fetches data, submits input, or manages asynchronous state, you must explicitly account for all four lifecycle states. *(Note: Static presentational components are exempt from artificial loading skeletons).*

1. **Loading State**:
   - Use shape-matching skeleton blocks (`animate-pulse` mimicking actual text lines and avatars).
   - Do NOT drop a solitary spinner in the center of an empty viewport.
2. **Empty State**:
   - Provide a helpful zero-data screen explaining what belongs here.
   - Include a primary call-to-action (e.g., "Create your first project").
3. **Error State**:
   - Provide clear inline recovery UI with an actionable retry button (e.g., "Failed to load invoices. [Try Again]").
   - Never fail silently or fall back to native `alert()`.
4. **Success State (Data-Rendered)**:
   - Clean, balanced presentation of the loaded data, free from layout shifts when transitioning from loading.

---

## 7. CSS & Tailwind Discipline

- **No Arbitrary Value Bloat**: Avoid random pixel classes (e.g., `w-[347px]`, `text-[#637281]`, `rounded-[13px]`). Anchor styles to design tokens (`w-80`, `text-muted-foreground`, `rounded-xl`). If custom sizing is strictly needed for alignment, define it via CSS variables or theme configuration.
- **Clean Z-Index Scale**: Never spray arbitrary values like `z-[9999]`. Use a standardized scale (`z-10`, `z-20`, `z-50`).
- **No Class Bloat**: Break 80+ character utility strings into small, focused subcomponents or compound styles.

---

## 8. Motion & Animation Restraint

- **Respect `prefers-reduced-motion`**:
  - Always provide a reduced-motion fallback: `motion-reduce:transition-none motion-reduce:animate-none`.
- **Hardware Acceleration**: Only animate CSS `transform` and `opacity`. Never animate layout-triggering properties (`width`, `height`, `top`, `margin`).
- **Snappy Micro-interactions**: Keep transition durations between 150ms and 250ms with `ease-out`. Avoid sluggish, floaty animations.
- **No Gratuitous Scroll Animations**: Do NOT wrap every card and heading in unprompted entrance animations.

---

## 9. Copywriting Authenticity (Anti-Buzzword Policy)

- **Banned AI Marketing Clichés**: Never write:
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

## 10. Editorial Restraint & Anti-Overengineering

When avoiding generic templates, models often over-compensate by making websites hyper-dense, overly technical, or structurally cluttered. Follow these rules:

1. **Zero Dynamic Layout Shift (CLS)**:
   - Dynamic text containers (e.g., tab content, active swatch descriptions) MUST reserve space using `min-h-*` tokens or CSS grid template rows so text variations never shift surrounding buttons or layout elements.
2. **Frameless Hierarchy (Banish Border & Container Soup)**:
   - Do NOT nest bordered cards inside bordered containers.
   - Let text, headlines, and actions sit **directly on the page surface**. Use whitespace and subtle tonal steps rather than enclosing every single item in a box.
3. **Audience-Centric Emotional Tone**:
   - Match the emotional frequency of the target demographic. Do not turn consumer apps or premium lifestyle brands into aerospace cockpits or cybersecurity consoles.
4. **Scope-Aware Metrics (No Marketing Metric Vomit)**:
   - *In Data Dashboards*: High data variance and technical metrics are encouraged (see Section 3).
   - *In Marketing / Landing Pages*: Do NOT dump unrequested technical specifications, decibel levels, or engineering jargon unless explicitly asked. Understated confidence is more effective than defensive metric vomiting.
5. **No Trivial Hover Zooms**:
   - Prohibit `hover:scale-105` or `group-hover:scale-110` on image cards. Use subtle opacity shifts or tranquil static presentation.
6. **No Generic Category Pill Tabs**:
   - Do not default to horizontal pill bars (`[All] [Item 1] [Item 2]`) over showcases like a generic blog. Present collections through curated editorial showcases, horizontal rails, or focused stages.
7. **Grid Uniformity Ban**:
   - Banning 3-column grids does NOT mean making 4 or 6 identical cards in a row. Use asymmetric arrangements, featured hero cards, or clean vertical flows.
8. **Card Simplicity & Typographic Restraint**:
   - Limit cards to at most **2 typographic roles** (e.g., headline + short supporting sentence).
   - Do not jam artificial numbers (`"01"`, `"Phase 1"`), timestamps, and 4 font styles into a single card.
9. **Visual-First Economy (No Encyclopedia Dumps)**:
   - Promotional pages are not user manuals. Limit sections to 1 primary headline (3-6 words) and 1-2 punchy sentences. Let imagery and whitespace do the work.
10. **Zero Chapter & Section Numbering**:
    - Never prefix sections with textbook labels like `"Section 01"`, `"Chapter 2"`, or `"03 / Architecture"`. Transitions should feel organic through typography and whitespace.

---

## Pre-Flight Verification Checklist

Before presenting frontend code or declaring a UI task complete, audit against this checklist:

- [ ] **No Emojis in UI**: Are all buttons, badges, headers, and navigation links 100% free of Unicode emojis?
- [ ] **No Broken Images**: Are all `<img>` tags pointing to verified URLs, inline SVGs, or initials fallbacks (no hallucinated paths)?
- [ ] **Semantic Buttons**: Are all clickable controls actual `<button>` or `<a>` tags (zero `<div onClick>`)?
- [ ] **Accessible Focus**: Can the interface be navigated with `Tab` and `Enter`/`Space` with clear `focus-visible` indicators?
- [ ] **Reduced Motion**: Are animations paired with `motion-reduce:transition-none`?
- [ ] **Contrast Compliant**: Does text meet WCAG 2.1 AA contrast requirements against its background?
- [ ] **Async Lifecycle Handled**: Did you account for Loading (Skeleton), Empty, Error (with retry), and Success states on data components?
- [ ] **Interactive States Covered**: Do interactive controls have distinct feedback for hover, focus-visible, active, and disabled?
- [ ] **No Dynamic Layout Shift**: Do dynamic text containers have reserved minimum heights to prevent surrounding jumps?
- [ ] **Frameless Hierarchy**: Is the layout free of unnecessary nested borders ("border soup")?
- [ ] **No Cheap Hover Zooms**: Are image cards free of `hover:scale-105` clichés?
- [ ] **No Section Numbering**: Are section titles free of textbook labels like "Section 01" or "Chapter 2"?
- [ ] **No Marketing Buzzwords**: Is the copy concise, authentic, and free of generic AI hype words?
