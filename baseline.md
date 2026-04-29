# Sera Website — Current-State Baseline

A frozen snapshot of the site as it stands on 2026-04-28. DESIGN.md describes the **target** system; this file documents the **current** code so the audit step has a concrete comparison surface.

---

## 1. File map

| File | Lines | Purpose |
|---|---|---|
| `index.html` | 1116 | Home / landing |
| `product.html` | 2772 | Product / features deep dive |
| `solutions.html` | 1472 | Industries (PT, dental, chiro, med spa, home services, real estate) |
| `pricing.html` | 1574 | Pricing tiers + comparison + FAQ |
| `security.html` | 867 | HIPAA, encryption, infrastructure, compliance |
| `css/styles.css` | 607 | Shared design tokens, nav, footer, button system, FAQ, reveal animations |
| `js/main.js` | 92 | Nav scroll, hamburger, scroll-reveal, FAQ accordion, stat counter, scroll-progress |
| `assets/icons/sera-icon.svg` | — | Logo glyph |

Each HTML file carries a single `<style>` block (~450 lines for `index.html`) holding page-specific styles in addition to the shared `css/styles.css`.

---

## 2. Current sections, by page

**index.html:** `hero` → `audio-demo` → `stats` → `integrations` → `how-it-works` → `call-flow` → `features` → `compare` → `showcase` → `trust-bar` → `industries` → `testimonials` → `case-studies` → `comparison` (before/after) → `pricing` → `faq` → `dark-cta` → footer.

**product.html:** `hero` → `how-it-works` → `features` → `scenarios` → `deep-dive` → `comparison` → `marquee-section` → `dark-cta` → footer.

**solutions.html:** `hero` → `industry-1..6` (PT, Dental, Med Spa, Chiro, Home Services, Real Estate) → `tech-stack` → `dark-cta` → footer.

**pricing.html:** `hero` → `pricing-section` → `includes-section` → `compare-section` → `faq-section` → `questions-cta` → footer.

**security.html:** `sec-hero` (dark) → `confidence` → `hipaa` → `encryption` → `call-flow` → `infrastructure` → `data-privacy` → `compliance-docs` → `security-faq` → `contact-security` → `dark-cta` → footer.

Common pattern: each page closes with a `dark-cta` (charcoal "Book a Demo") and the dark footer. Sections are well-named and consistently classed.

---

## 3. Current design tokens (as declared in `css/styles.css`)

```css
--green: #0D9B6A;
--green-hover: #0B8A5E;
--green-light: #ECFDF5;
--green-glow: rgba(13, 155, 106, 0.12);
--green-dark: #064E3B;
--teal: #10B981;
--dark: #0A1A14;
--dark-mid: #0D1F17;
--text-primary: #111827;
--text-secondary: #6B7280;
--text-muted: #9CA3AF;
--bg-white: #FFFFFF;
--bg-off-white: #F9FAFB;
--bg-green-tint: #F0FDF4;
--border: rgba(0, 0, 0, 0.06);
--shadow-sm / md / lg / glow (4 steps)
--radius-sm 8px / md 12px / lg 16px / pill 100px
--font-heading: 'Cabinet Grotesk', system-ui, sans-serif;
--font-body: 'Inter', system-ui, sans-serif;
--font-mono: 'JetBrains Mono', 'IBM Plex Mono', monospace;
```

**Loaded fonts (Google Fonts):** Inter (400/500/600/700), JetBrains Mono (400/500). Some pages also import IBM Plex Mono.

**Critical bug:** `--font-heading` is declared as `'Cabinet Grotesk'` but **Cabinet Grotesk is never imported**. All headings silently fall back to system-ui. The "elegant geometric grotesk" the system was designed around does not actually render.

**Off-token color sprawl:** index.html alone contains 50+ ad-hoc hex literals (`#EDE9FE`, `#F5F3FF`, `#FF7A59`, `#1a1a2e`, `#2d2b55`, `#FBBF24`, `#FEF2F2`, etc.) embedded in the page-level `<style>` block — outside the token system entirely. A handful are macOS-window-chrome oranges/yellows/reds (`#FF5F57`, `#FEBC2E`, `#28C840`) used in product mockups. Many others are one-off section accents.

---

## 4. Interactive behaviors (`js/main.js`)

| Behavior | Trigger | Implementation |
|---|---|---|
| Nav scrolled state | `window.scrollY > 50` | Toggles `.nav.scrolled` for blur-backdrop + hairline |
| Hamburger menu | Click on `#hamburger` | Toggles `.nav__mobile-menu.open`; closes on link click |
| Scroll reveal | `IntersectionObserver` (10% threshold, -40px rootMargin) | Adds `.visible` to any `.reveal` once in viewport. Staggered children via `.reveal-group` (80ms steps, max 6) |
| FAQ accordion | Click on `.faq-item__question` | Single-open: closes all siblings, toggles `.open` on clicked. Pure CSS height transition (max-height 0 → 500px) |
| Stat counter | `IntersectionObserver` (30% threshold) | Animates `[data-count]` 0 → target over 1500ms with ease-out cubic; reads `data-prefix` / `data-suffix` |
| Scroll progress bar | `scroll` event | Sets width % on `#scrollProgress` based on scrollY / scrollHeight |

All passive listeners. No third-party JS. ~92 lines, well-scoped.

---

## 5. Current component patterns

| Component | Token chain | Notes |
|---|---|---|
| Primary CTA | `.btn.btn--primary` — `var(--green)` bg, white text, pill radius | Pill, not rectangle. Hover scales 1.02 + shifts to `--green-hover`. |
| Outline CTA | `.btn.btn--outline` — white bg, `var(--border)` 1px, `--text-primary` text | Pill. |
| White CTA | `.btn.btn--white` — used on dark sections | Pill. |
| Ghost CTA | `.btn.btn--ghost` — transparent on dark, 25%-white border | Pill. |
| Card | `.card` — white bg, 1px border, 12px radius, `--shadow-sm` | Translate-up on hover. |
| Badge / chip | `.badge.badge--green` or `--teal` — green-light bg, green text, pill | 0.7rem font, used for HIPAA, "Live", trust pills. |
| Eyebrow | `.eyebrow` — green text, ALL CAPS, 0.1em tracking, 0.8125rem | Already on-brand for the target system. |
| Section headline | `.section-headline` — Cabinet Grotesk (broken), 800 weight, clamp(2rem, 4vw, 2.8rem) | Falls back to system-ui. |
| Section sub | `.section-sub` — gray, 1.0625rem, 1.65 leading, max-width 640px | Reasonable measure. |
| FAQ item | `.faq-item` — bottom border, chevron, max-height transition | Single-open behavior. |
| Hero trust pills | Inline SVG check + label, 4 horizontally | Used on home hero. |

---

## 6. Gaps: current → target

### 6.1 Color
| Token | Current | Target | Gap severity |
|---|---|---|---|
| Brand green | `#0D9B6A` (emerald, vivid) | `#2d7d5f` (healthcare green, muted) | High — visible saturation drop, more "medical" |
| Dark | `#0A1A14` (near-black, green-tinted) | `#1a1a1a` (charcoal, neutral) | Medium — current is greener than target |
| Page background | `#FFFFFF` (pure white) | `#fbf9f3` (ivory, warm) | High — current violates the No-Pure rule |
| Text primary | `#111827` (slate-900) | `#1a1a1a` (charcoal) | Low — visually similar |
| Border | `rgba(0,0,0,0.06)` (cool) | `#e6e3da` (warm hairline) | Medium — borders read cool today |
| Accent (data) | not defined | `#0066cc` (clinical blue) | New — must be added |
| Warm accent | `#FF7A59` / `#E97025` (one-offs in HTML) | `#c85a2a` (terracotta, system token) | High — current usage is unsystematic |

### 6.2 Typography
| Element | Current | Target | Gap severity |
|---|---|---|---|
| Display / Headline | Cabinet Grotesk **declared but not loaded** → system-ui fallback | Cormorant Garamond (serif, editorial) | Critical — register flip from grotesk to serif |
| Body | Inter | Instrument Sans | Medium — both modern sans, swap is mechanical |
| Data / Mono | JetBrains Mono | Space Grotesk | Medium — Space Grotesk is sans-mono hybrid, different feel |
| Heading weight | 800 | 500 | High — current is bold/loud, target is restrained |

### 6.3 Shape / form
| Property | Current | Target | Gap severity |
|---|---|---|---|
| Button radius | `--radius-pill` (100px) | `--rounded.sm` (4px sharp rectangle) | High — register flip from friendly-pill to operator-rectangle |
| Card radius | 12px | 8px | Low — minor refinement |
| Card border | 1px `rgba(0,0,0,0.06)` | 1px `#e6e3da` | Low — color shift only |
| Hover behavior | Translate -2px + shadow grows | Translate -1px + state-hover shadow appears | Low — minor refinement |

### 6.4 Off-token sprawl (highest-impact gap)
- `index.html` style block: ~50 hex literals not derived from tokens.
- `product.html`, `solutions.html`, `security.html`: similar sprawl in their inline `<style>` blocks (not catalogued line-by-line — flagged as a class of issue for the audit).
- Any color migration must clean these up, not just edit the `:root` block.

### 6.5 Missing system pieces
- **Cormorant Garamond** font import (not loaded anywhere).
- **Instrument Sans** font import (not loaded; would replace Inter).
- **Space Grotesk** font import (not loaded; would replace JetBrains Mono).
- **Clinical Blue** as a token (no equivalent today).
- **Ivory / Paper / Rule** warm-tinted neutral tokens.
- **Focus ring** treatment — no `:focus-visible` styling found in shared CSS.
- **`prefers-reduced-motion`** respect — neither `main.js` nor `css/styles.css` checks it.

### 6.6 Architectural notes for the audit
- The shared CSS file is small and clean (607 lines, well-organized). Token migration is a localized edit.
- Page-level `<style>` blocks are the real liability. Most AI-slop will surface there.
- All 5 pages share `nav` and `footer` markup and styling. A single edit to `css/styles.css` will land across the site.
- The marketing site is static HTML (no build pipeline) — fonts can be added by `<link>` in `<head>` without tooling.

---

## 7. What to read next

- **DESIGN.md** — the target system the audit will measure against.
- **PRODUCT.md** — strategic register, anti-references, design principles.
- **IMPECCABLE_STRATEGY.md** — the user's original positioning and messaging brief.
