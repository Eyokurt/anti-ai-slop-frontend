# Anti-AI-Slop Frontend Craftsmanship

> Production-grade frontend and UI craftsmanship guidelines for AI coding assistants. Eliminates generic "AI slop" and enforces clean aesthetics, strict semantic HTML, complete 4-state components, SVG-only iconography, authentic copy, and modern CSS/Tailwind discipline.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Format: Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-success.svg)](SKILL.md)

---

## What is "AI Slop" in Frontend Code?

LLMs and AI coding assistants naturally gravitate toward predictable, generic, and uninspired frontend defaults:
- **Neon purple/violet glowing drop shadows** on pitch-black surfaces
- **Emoji spam** embedded directly into buttons, badges, and headers (`<button>🚀 Get Started</button>`, `<span>✨ New</span>`)
- **Broken placeholder file paths** (`<img src="/placeholder.svg" />`, `src="/avatar.jpg"`)
- **Unsemantic div soup** with inaccessible `<div onClick={...}>` handlers
- **Happy-path blindness**: Omitting loading skeletons, empty zero-data states, and error recovery screens
- **AI marketing buzzwords**: *"Unlock the power of..."*, *"Elevate your experience"*, *"Seamless integration"*
- **Generic 3-card layouts** repeated on every section

This skill enforces uncompromising engineering and visual standards to make AI-generated frontend interfaces feel human-crafted, resilient, and ready for production.

---

## Core Principles & Directives

### 1. Absolute Emoji Prohibition & Strict Iconography
- **Zero Emojis in UI**: Never use Unicode emojis in buttons, status badges, headers, toasts, or empty states.
- **Monochromatic Vector SVGs**: Use dedicated icon libraries (Lucide React, Heroicons, Phosphor, Radix, Tabler).
- **Color Inheritance**: Icons must inherit color via `currentColor` to harmonize with theme shifts.
- **Accessibility**: Decorative icons must have `aria-hidden="true"`; standalone icon buttons require `aria-label`.

### 2. Placeholder & Mock Asset Hygiene
- **No Hallucinated Paths**: Never link to non-existent local files.
- **Avatar Initials Fallback**: Always implement initials fallbacks (e.g., `<div className="size-8 rounded-full ...">EK</div>`) instead of raw broken `<img>` tags.
- **Form Placeholders**: Input placeholders are strictly for syntax/format hints (e.g., `alex@company.com`), never labels or instructions.
- **Zero Lorem Ipsum**: Write realistic, domain-specific copy matching the application context.

### 3. Aesthetic & Visual Philosophy
- **Grounded Base Palette**: Use off-blacks (`#0a0a0a`, `#121212`, `#0f172a`), refined neutral tones, and at most **one** deliberate brand accent.
- **Subtle Surface Steps**: Separate elevation and hierarchy through subtle contrast shifts rather than heavy 1px borders everywhere.
- **Viewport Stability**: Use `min-h-dvh` instead of `100vh` or `h-screen` to prevent layout bouncing on mobile browser URL bars.
- **Typography Polish**: Use `text-wrap: balance` on headings, `text-wrap: pretty` on body copy, and `tabular-nums` on metric counters and numeric tables.

### 4. Semantic HTML & Accessibility
- **Strict Div-Button Ban**: Interactive elements must be semantic `<button type="button">` or `<a href="...">`.
- **Keyboard Navigation**: Clear, visible `:focus-visible` rings on all interactive elements.
- **Form Structure**: Proper `<label htmlFor="...">` associations and grouped `<fieldset>` blocks.

### 5. Mandatory 4-State Component Architecture
Every interactive or data-driven component must implement all four core states:
1. **Loading State**: Content-shaped skeleton loaders (`animate-pulse`) mirroring real content.
2. **Empty State**: Friendly zero-data visual explaining what belongs here + actionable primary CTA.
3. **Error State**: Non-blocking inline error banner with an explicit retry button.
4. **Interactive States**: Distinct `:hover`, `:focus-visible`, `:active`, and `:disabled` feedback.

### 6. CSS & Tailwind Discipline
- **Token Consistency**: Stick strictly to design tokens (`w-80`, `text-muted-foreground`, `rounded-xl`). Avoid arbitrary pixel hacks like `w-[347px]`.
- **Z-Index Scale**: Maintain a clean, predictable layering hierarchy (`z-10`, `z-20`, `z-50`).

### 7. Motion & Animation Restraint
- Only animate GPU-accelerated properties (`transform`, `opacity`). Never animate layout triggers (`width`, `height`, `margin`).
- Keep transitions short and snappy (150ms–250ms with `ease-out`).

### 8. Copywriting Authenticity
- Banned AI marketing clichés: *"Unlock the power of"*, *"Supercharge your workflow"*, *"Next-gen solution"*, *"Elevate your experience"*.
- Replace buzzwords with clear, active, and concrete product capabilities.

---

## Installation & Usage

### Google Antigravity / Gemini CLI
Clone or copy this skill into your Antigravity skills directory:

```powershell
# Global skill directory
git clone https://github.com/Eyokurt/anti-ai-slop-frontend.git "$HOME\.gemini\config\skills\anti-ai-slop-frontend"
```

Or copy the `SKILL.md` directly into your workspace:
```bash
mkdir -p .gemini/skills/anti-ai-slop-frontend
cp SKILL.md .gemini/skills/anti-ai-slop-frontend/
```

### Claude Code
Add the skill to your project's `.claude/skills` directory:

```bash
mkdir -p .claude/skills/anti-ai-slop-frontend
cp SKILL.md .claude/skills/anti-ai-slop-frontend/SKILL.md
```

Or include a directive in your `CLAUDE.md`:
```markdown
Whenever writing frontend code or UI components, follow the anti-ai-slop-frontend craftsmanship rules in .claude/skills/anti-ai-slop-frontend/SKILL.md.
```

### Cursor & Windsurf
Add the rules from [`SKILL.md`](SKILL.md) to `.cursorrules` or `.windsurfrules`.

---

## Pre-Flight Verification Checklist

Before presenting frontend code or declaring a UI task complete:

- [ ] **No Emojis**: Are all buttons, badges, headers, and toasts 100% free of Unicode emojis?
- [ ] **No Broken Images**: Are all `<img>` tags pointing to real files, seeded placeholders, or initial fallbacks?
- [ ] **No Div Buttons**: Are all clickable controls actual `<button>` or `<a>` tags?
- [ ] **Focus Visible**: Can the entire component be navigated and operated using only `Tab` and `Enter`/`Space`?
- [ ] **4 States Handled**: Did you provide Loading (Skeleton), Empty, Error, and Active states?
- [ ] **No Generic Buzzwords**: Is the copy clear, professional, and free of AI marketing clichés?
- [ ] **No Arbitrary CSS Hacks**: Are styles anchored to design tokens rather than random bracketed pixel values?

---

## License

This project is licensed under the [MIT License](LICENSE).

