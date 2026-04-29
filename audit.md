# Sera — Technical Audit Report

**Date:** 2026-04-28
**Scope:** index.html, product.html, solutions.html, pricing.html, security.html, css/styles.css, js/main.js
**Standard:** WCAG 2.1 AA (per PRODUCT.md), DESIGN.md target system

---

## Audit Health Score

| # | Dimension | Score | Key Finding |
|---|-----------|-------|-------------|
| 1 | Accessibility | **1 / 4** | Zero focus styling site-wide; no `prefers-reduced-motion`; sub-44px nav touch targets |
| 2 | Performance | **3 / 4** | Vanilla, fast first paint; missing lazy-load + multiple infinite animations |
| 3 | Theming | **1 / 4** | Token system exists but bypassed: 43+32+31 unique hexes in three pages outside tokens; `--font-heading` declared but never imported |
| 4 | Responsive | **2 / 4** | Two breakpoints; nav CTA fails 44×44 touch target; no fluid type below 320px |
| 5 | Anti-Patterns | **1 / 4** | Hero-metric template, gradient text, side-stripe borders, 13+ vague CTAs, 97 em dashes |
| **Total** | | **8 / 20** | **Poor — major overhaul** |

---

## Anti-Patterns Verdict

**Pass / Fail: FAIL — looks AI-generated.**

A skilled reader would call this site as machine-assembled within 3 seconds of scrolling. The tells:

1. **The hero-metric template, on rails.** `index.html:606-609` — four stat cards in a perfectly even grid: gigantic gradient-text number, small label, supporting description, optional pulse. This is the single most-cloned SaaS layout of the last five years. (Banned in DESIGN.md §6, line "Don't use bento-box grids…").
2. **Gradient text** on the hero accent (`.hero__headline .green`) and stat numbers (`.stat__number`). Linear-gradient `#0D9B6A → #10B981`, `-webkit-background-clip: text`. Banned in DESIGN.md and in the parent skill's absolute bans.
3. **Side-stripe borders** (`border-left: 4px solid …`) used as accent on three callout components: `.audio-demo__callout-box`, `.comparison__card--without`, `.comparison__card--with`. Absolute-banned in DESIGN.md.
4. **Vague CTAs** as the *primary* button copy: "Get Started →" appears on every pricing tier and the final CTA on three pages; "Read More →" closes every case-study card; "Learn More →" closes every industry card on solutions.html. PRODUCT.md anti-references called these out by name.
5. **Em dashes flooding body copy** — 97 occurrences across the five pages. The shared design laws explicitly ban em dashes in copy.
6. **Gradient-shift animations on fixed elements** (`product.html:82,96`) — a top-bar that perpetually animates a green-to-teal gradient. Two of the absolute bans (gradient + decorative motion) compounded.

The visual language is "category-reflex healthcare SaaS" — emerald + bright-teal + pure-white + pill buttons + identical card grids. The exact thing PRODUCT.md says we explicitly reject.

---

## Executive Summary

- **Audit Health Score:** 8 / 20 (Poor — major overhaul)
- **Issues found:** 6 P0 / 11 P1 / 7 P2 / 5 P3
- **Top critical issues:**
  1. **No focus styling anywhere on the site.** Every keyboard user sees only the browser default outline (or nothing in browsers that suppress it on `tabindex` elements). WCAG 2.4.7 violation.
  2. **No `prefers-reduced-motion` respect.** Five infinite animations (pulseDot, pulseRing, pulseSoon, pulseAnswer, phonePulse, gradientShift ×2) plus scroll-reveal and stat counters fire regardless of user preference. WCAG 2.3.3 violation.
  3. **Hero-metric template + gradient text** on `.stat__number` — the canonical AI-slop tell.
  4. **`Cabinet Grotesk` declared but never imported.** Every headline silently falls back to system-ui. The "elegant geometric grotesk" the design was built around is not actually rendering.
  5. **Token system bypassed.** index.html alone contains 43 unique hex colors outside the `:root` token block. Migrating `--green` will not migrate the site — the inline `<style>` blocks must also be cleaned.
  6. **`href="#"` placeholder links throughout** — 71 across the site. Many are placeholders that go nowhere. They are tab-focusable and will route screen readers to the top of the page on activation, breaking flow.

---

## Detailed Findings by Severity

### P0 — Blocking (fix immediately)

**[P0] No focus styling on any interactive element**
- **Location:** css/styles.css (entire file — zero `:focus` or `:focus-visible` rules)
- **Category:** Accessibility
- **Impact:** Keyboard users cannot see which element has focus on most browsers. Catastrophic for screen-magnifier and motor-impairment users.
- **WCAG/Standard:** WCAG 2.4.7 Focus Visible (Level AA)
- **Recommendation:** Add a global `:focus-visible` rule with the `state-focus` shadow from DESIGN.md (`box-shadow: 0 0 0 3px rgba(45,125,95,0.20)`). Override per-component where needed.
- **Suggested command:** `$impeccable harden`

**[P0] No `prefers-reduced-motion` respect**
- **Location:** css/styles.css (`.reveal` transition); js/main.js (counter animation, scroll-reveal observer); index.html `<style>` (5 infinite animations); product.html `<style>` (2 infinite gradient animations + marquee)
- **Category:** Accessibility
- **Impact:** Vestibular-disorder users will experience nausea or migraine triggers. Also fails the user's explicitly stated PRODUCT.md accessibility requirement.
- **WCAG/Standard:** WCAG 2.3.3 Animation from Interactions (Level AAA, but commonly required at AA in healthcare)
- **Recommendation:** Wrap all keyframe animations and `.reveal` transitions in `@media (prefers-reduced-motion: no-preference)`. Add a global override that disables `animation-iteration-count: infinite` under reduced motion.
- **Suggested command:** `$impeccable harden`

**[P0] Hero-metric template + gradient text on stats section**
- **Location:** index.html:163, 606-609 (`.stat__number`)
- **Category:** Anti-Pattern
- **Impact:** The single strongest AI-slop signal on the site. Banned in DESIGN.md. Reads as cloned-template to anyone evaluating the brand.
- **Recommendation:** Replace `.stat__number` gradient with solid `--charcoal` color in Space Grotesk; restructure the stats block away from a 4-column even grid. Per DESIGN.md, metrics belong in `metric-block` token paired with a Body-Small caption, used contextually inside testimonial cards rather than as a standalone hero.
- **Suggested command:** `$impeccable distill` then `$impeccable typeset`

**[P0] Gradient text on hero accent**
- **Location:** index.html:50 (`.hero__headline .green`)
- **Category:** Anti-Pattern
- **Impact:** Absolute-banned. The green-to-teal gradient on "Patient's Call" reads as decoration rather than meaning.
- **Recommendation:** Replace with a solid `--healthcare-green` color. Emphasis through size or weight, not color gradient.
- **Suggested command:** `$impeccable typeset`

**[P0] Cabinet Grotesk declared but never imported**
- **Location:** css/styles.css:38 (`--font-heading: 'Cabinet Grotesk', system-ui, sans-serif`); zero `<link>` to a Cabinet Grotesk source in any HTML file
- **Category:** Theming
- **Impact:** Every `h1`-`h6`, `.section-headline`, `.nav__logo-text`, `.footer__logo` falls back to system-ui silently. The visual identity of the entire site is the wrong typeface.
- **Recommendation:** Either import Cabinet Grotesk via `<link>` OR (per the DESIGN.md migration) replace with Cormorant Garamond and Instrument Sans. Step 5 of the multi-step plan is the right place — but the bug should be flagged in the migration commit so it isn't masked by the swap.
- **Suggested command:** `$impeccable typeset`

**[P0] Side-stripe borders as accent (4px colored left border)**
- **Location:** index.html:154 (`.audio-demo__callout-box`), :383 (`.comparison__card--without`), :384 (`.comparison__card--with`)
- **Category:** Anti-Pattern
- **Impact:** Absolute-banned. The "border-left: 4px solid color" pattern is one of the most-templated SaaS layouts.
- **Recommendation:** Per DESIGN.md, rewrite with full borders + background tints (Healthcare-Green-Tint or Signal-Danger-Tint). Lead with a leading icon or eyebrow label; never with a colored stripe.
- **Suggested command:** `$impeccable layout`

### P1 — Major (fix before release)

**[P1] Vague primary CTAs across all pricing tiers and final CTAs**
- **Location:** Every "Get Started →", "Read More →", "Learn More →" instance — minimum 13 across the five pages
- **Category:** Anti-Pattern + UX writing
- **Impact:** PRODUCT.md anti-references call these out explicitly. Vague CTAs reduce conversion and read as generic SaaS template.
- **Recommendation:** Replace with action-and-outcome copy: "Start a 14-Day Trial" (low-friction tier), "Talk to Sales" (enterprise tier), "Read the Case Study" (case-study cards), "See Physical Therapy in Detail" (industry cards), "Book a Demo →" (final CTA). Per DESIGN.md §5 Buttons, every CTA names what happens.
- **Suggested command:** `$impeccable clarify`

**[P1] Em dashes pervasive in body copy (97 instances)**
- **Location:** All five HTML files (index 23, pricing 29, solutions 17, product 15, security 13)
- **Category:** Anti-Pattern (copy)
- **Impact:** The shared design laws and DESIGN.md §6 ban em dashes in body copy. The "—" character is the clearest written-by-AI tell in the post-2023 era.
- **Recommendation:** Replace each with comma, colon, semicolon, period, or parentheses depending on context. The single allowed instance is the testimonial signature ("— Dr. Sarah Chen, …") with Terracotta underline.
- **Suggested command:** `$impeccable clarify`

**[P1] Five+ infinite animations on the home page**
- **Location:** index.html `<style>` — `pulseDot` (×2), `pulseRing`, `pulseSoon`, `pulseAnswer`, `phonePulse` (in product.html); product.html — `gradientShift` (×2)
- **Category:** Performance + Accessibility
- **Impact:** Continuous CPU/GPU load even on idle tabs. Compounds the reduced-motion violation.
- **Recommendation:** Remove `pulseSoon` and `pulseAnswer` outright (they are decorative on inactive UI elements). Keep `pulseDot` only on the live-status indicator and gate behind `prefers-reduced-motion: no-preference`. Remove the gradient-shift top-bar (gradient + decoration violation in one).
- **Suggested command:** `$impeccable distill` then `$impeccable harden`

**[P1] `href="#"` placeholder anchors that route nowhere**
- **Location:** All five HTML files (16+14+10+11+20 = 71 instances). Many resolve to `#dark-cta` etc., but a meaningful subset are bare `#` placeholders (login link, case-study Read More, industry Learn More, etc.)
- **Category:** Accessibility
- **Impact:** Screen-reader users activating these are jumped to the top of the page with no announcement. Tab-focusable but functionally broken.
- **Recommendation:** Replace placeholders with real targets, or convert to `<button type="button">` with no-op handlers, or remove entirely until destinations exist. Add `aria-label` describing intent.
- **Suggested command:** `$impeccable clarify`

**[P1] Nav CTA touch target ~31px on mobile (fails WCAG 2.5.5)**
- **Location:** css/styles.css:349-358 (`.nav__cta`) — `padding: 0.55rem 1.5rem`, font 0.85rem
- **Category:** Responsive + Accessibility
- **Impact:** Mobile users (the primary audience per PRODUCT.md — "operator scrolling at 7am between patients on a phone") can't reliably tap the primary conversion CTA.
- **WCAG/Standard:** WCAG 2.5.5 Target Size (Level AAA, but standard for healthcare per the user's accessibility note)
- **Recommendation:** Increase mobile padding to ≥`0.75rem 1.5rem` to clear 44px. Consider a small-screen variant that renders the full `.btn--primary` instead of a compact form.
- **Suggested command:** `$impeccable adapt`

**[P1] Mobile menu link touch targets ~36px**
- **Location:** css/styles.css:402-408 (`.nav__mobile-menu a`) — `padding: 0.75rem 0`
- **Category:** Responsive + Accessibility
- **Impact:** Same as above — mobile-first audience cannot reliably tap menu items.
- **Recommendation:** Increase to `padding: 0.9rem 1rem` minimum to clear 44px with body type.
- **Suggested command:** `$impeccable adapt`

**[P1] Token system bypassed — 43 unique hexes in index.html alone**
- **Location:** index.html `<style>` block (lines ~1-454); product.html `<style>` (32 unique); solutions.html `<style>` (31 unique)
- **Category:** Theming
- **Impact:** Migrating `--green` to `#2d7d5f` will leave 100+ stale colors across the inline blocks. The token system is decorative if it's not the source of truth.
- **Recommendation:** During Step 5 (token migration), every inline-styled hex must be either (a) replaced with a token reference, (b) consciously preserved as a one-off (and documented with a comment), or (c) deleted as dead code. Consider extracting page-level inline `<style>` into per-page CSS files for maintainability.
- **Suggested command:** `$impeccable polish` (after Step 5)

**[P1] Pure-white surfaces violate the No-Pure rule**
- **Location:** css/styles.css:150 (`.btn--outline` bg), :170 (`.btn--white` bg), :297 (`.nav__dropdown-menu` bg), plus dozens of inline-style instances using `#fff` / `#FFFFFF`
- **Category:** Theming
- **Impact:** DESIGN.md's No-Pure rule says neither `#000` nor `#fff` is allowed. Pure values read as default; the brand reads as default.
- **Recommendation:** Swap to `--ivory` (`#fbf9f3`) for all page backgrounds and `--pure-white` (`#ffffff` — explicitly declared as a token for *card-on-paper* / inputs only) where pure white is structurally needed. Most inline `#fff` cases are page surfaces and should switch to ivory.
- **Suggested command:** `$impeccable colorize`

**[P1] Pure black on text inside yellow badges**
- **Location:** index.html:210 (`.flow-tab__soon`), :231 (`.feature-card__soon`)
- **Category:** Theming
- **Impact:** No-Pure rule. Also the "Soon" badge on a yellow background with pulsing animation reads as gimmicky for a healthcare buyer.
- **Recommendation:** Replace with `--charcoal` (`#1a1a1a`) on a tinted yellow (`--signal-warning-tint`); kill the pulse animation. Reconsider whether "Soon" labels belong on a marketing page at all.
- **Suggested command:** `$impeccable colorize`

**[P1] Heading hierarchy flat below `h2`**
- **Location:** index.html (1 h1, 13 h2, no h3+), pricing.html (1 h1, 4 h2), product.html (1 h1, 6 h2, 4 h3), solutions.html (1 h1, 8 h2). Only security.html has proper h1→h2→h3→h4.
- **Category:** Accessibility
- **Impact:** Screen-reader navigation by heading is impoverished — users cannot drill into a section's substructure. Long pages like solutions.html (8 industry sections) are a flat list to assistive tech.
- **Recommendation:** Promote each industry's title to `<h3>` under the section's `<h2>`. Same for case-study titles, FAQ questions (currently styled as headings but rendered as `.faq-item__question-text` divs — should be `<h3>`).
- **Suggested command:** `$impeccable harden`

**[P1] Identical case-study card grid (3 cards, same structure, same CTA copy)**
- **Location:** index.html:865, 869, 873 — three `.case-card` blocks with identical pattern (tag, title, "Read More →")
- **Category:** Anti-Pattern
- **Impact:** Per PRODUCT.md anti-references, the "identical card grid" is a category tell. With three cards it's borderline; a fourth pushes into clear violation.
- **Recommendation:** Vary the cards: lead one with a 3-line metric block, one with a quote, one with a clinic photo (post-PHI-scrub) and a name. Different hierarchies per card breaks the templated read.
- **Suggested command:** `$impeccable bolder` or `$impeccable layout`

**[P1] No lazy loading on images (small but free win)**
- **Location:** All five HTML files have one `<img>` (the logo). Currently no `loading="lazy"` and no `width`/`height` to reserve layout.
- **Category:** Performance
- **Impact:** Minor (only 5 images, mostly above-fold logos). Nonetheless free CLS prevention.
- **Recommendation:** Add `width` and `height` attributes to the logo for layout reservation; `loading="lazy"` is not needed for above-fold but doesn't hurt to add `decoding="async"`.
- **Suggested command:** `$impeccable optimize`

### P2 — Minor (next pass)

**[P2] `<a>` tags used for actions where `<button>` is correct**
- **Location:** All `.btn` elements styled as buttons but rendered as `<a href="#…">`
- **Category:** Accessibility
- **Impact:** Keyboard users get the wrong activation behavior (Enter triggers, Space does not on `<a>`). Screen readers announce as "link" when they're effectively buttons.
- **Recommendation:** Convert non-navigation actions (form submits, modal openers, scroll-to triggers) to `<button type="button">` with proper handlers.

**[P2] Pulse animations on decorative elements**
- **Location:** index.html — `.hero__badge-dot`, `.hero-card__dot`, `.audio-player__pulse.active`
- **Category:** Anti-Pattern (motion)
- **Impact:** "Live" pulse dots on static badges read as ornament, not signal.
- **Recommendation:** Keep pulse only on the audio-player active state; replace badge dots with a static colored dot.

**[P2] `transition: all 0.2s ease` used widely**
- **Location:** css/styles.css buttons, cards, nav links
- **Category:** Performance + craft
- **Impact:** `transition: all` opts every property in. Cheap but imprecise; can trigger layout-property animations if a property changes during interaction.
- **Recommendation:** List explicit properties: `transition: background 150ms cubic-bezier(0.165, 0.84, 0.44, 1), transform 150ms cubic-bezier(0.165, 0.84, 0.44, 1)`. Per DESIGN.md, ease-out-quart is the system curve.

**[P2] No `viewport-fit=cover` on viewport meta**
- **Location:** All five HTML files
- **Category:** Responsive (iOS notch)
- **Impact:** Sticky nav clipped under iOS Safari notch on newer devices.
- **Recommendation:** Update to `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">` and use `env(safe-area-inset-*)` in nav styling.

**[P2] No `print` stylesheet**
- **Location:** css/styles.css
- **Category:** Polish
- **Impact:** Pricing/security pages are likely-to-be-printed reference pages.
- **Recommendation:** Minimal `@media print` rule that strips dark-CTA section, footer chrome, and resets backgrounds.

**[P2] Body text color `--text-secondary` for default body is too light at #6B7280 on cream**
- **Location:** css/styles.css:51-58 (body color)
- **Category:** Accessibility (contrast)
- **Impact:** `#6B7280` on `#fff` measures 4.83:1 — passes 4.5:1 by 0.33. After ivory migration (`#fbf9f3`) the ratio drops below 4.5:1 because ivory is darker than pure white. Borderline at AA.
- **Recommendation:** Use `--charcoal` (`#1a1a1a`) for primary body text per DESIGN.md, reserve `--graphite` (`#5a5a5a`) — which sits at 7.0:1 on ivory — for secondary copy.

**[P2] FAQ answer hidden behind `max-height: 500px`**
- **Location:** css/styles.css:579 (`.faq-item.open .faq-item__answer { max-height: 500px }`)
- **Category:** Responsive + content
- **Impact:** FAQ answers longer than ~500px (any answer with a code block, list, or long paragraph) get clipped on open.
- **Recommendation:** Switch to `max-height: 1500px` *or* use grid-template-rows transition (`grid-template-rows: 0fr` → `1fr`) for content-aware accordion.

**[P2] Mobile menu bg uses `rgba(255,255,255,0.98)` over `--bg-white`**
- **Location:** css/styles.css:389
- **Category:** Theming
- **Impact:** When the user migrates to ivory page bg, the mobile sheet will visibly disagree with the rest of the page.
- **Recommendation:** Use `var(--bg-white)` token (post-migration: `--ivory`) so the value is centrally controlled.

### P3 — Polish

**[P3] No `<title>` content audit performed** — verify SEO titles match the new positioning ("Answer Every Call. Keep the Revenue.").

**[P3] No structured data (`<script type="application/ld+json">`)** — Healthcare practices may be ranked higher with `Product` + `Organization` schema.

**[P3] Open Graph + Twitter card meta missing on most pages** — social shares will use first paragraph as preview, not a curated description.

**[P3] No favicon for dark-mode browsers** — `favicon.svg` exists; consider adding `<link rel="icon" media="(prefers-color-scheme: dark)" …>`.

**[P3] `box-shadow: 0 0 60px var(--green-glow)`** (`--shadow-glow` token) — defined but unused. Dead token.

---

## Patterns & Systemic Issues

1. **Hard-coded hex sprawl in inline `<style>` blocks.** index.html (43), product.html (32), solutions.html (31) carry the bulk of the design debt. Migrating only `:root` tokens will leave the site visually unchanged in many places. **The token migration must be a search-and-replace pass through the inline blocks, not a `:root` edit.**

2. **Vague-CTA copy as a default reflex.** Every primary action button reads "Get Started," "Read More," or "Learn More." This is the AI-slop tell most directly in the user's stated anti-references. Fixing it requires a per-section copy pass, not a global find-replace.

3. **Decorative motion as a default reflex.** Six unique `@keyframes` rules across the site, most infinite, none reduced-motion-aware. Healthcare brand gravity wants stillness; this site reads as restless.

4. **Pure-white as the default surface.** The CSS shows the value `#fff` literally as the brand fill (`--bg-white: #FFFFFF`). The migration to ivory (`#fbf9f3`) is mechanical but must touch every place the literal `#fff`/`#FFFFFF` appears, not just the token.

5. **Heading-hierarchy flatness.** Outside security.html, sections do not nest. This is a content-architecture issue as much as a markup issue — section subgroups need real titles.

6. **Token name drift between current and target.** `--green` (current) → `--healthcare-green` (target); `--dark` → `--charcoal`; `--text-primary` → `--charcoal`; `--bg-white` → `--ivory`. Step 5's CSS rewrite has to land all of these atomically or risk half-migrated states.

---

## Positive Findings

What's working — preserve these:

- **Clean shared `css/styles.css` (607 lines).** The base file is well-organized, tokenized, and small. Most damage is in inline `<style>` blocks, not the shared CSS.
- **Vanilla JS, ~92 lines, zero dependencies.** Nav scroll, scroll-reveal, FAQ accordion, stat counter, scroll-progress are all small, passive, and well-scoped.
- **Section-eyebrow pattern is already on-brand.** `.eyebrow` ALL-CAPS green pattern matches DESIGN.md §5 Section Eyebrow exactly. This component survives migration.
- **Hero badge with status dot + "HIPAA-Compliant Platform" copy** is the right register and only needs color/font swap.
- **Trust pills in the hero with checkmark + outcome copy** ("Answers Calls Instantly," "Books Into Your System") match the outcomes-not-features rule from PRODUCT.md.
- **Stats section copy** is outcome-focused — "Missed Calls While Live: 0," "Reclaimed for patient care" — only the visual treatment is broken (gradient text + hero-metric template).
- **Specific company names in social proof** ("Summit Physical Therapy, Bright Smile Dental, Valley Chiropractic") follow the "specificity for trust" principle.
- **Mobile menu and hamburger are properly wired** (aria-label exists on hamburger).
- **Every page closes with the dark-cta + footer pattern** — consistent architecture across the site, which makes Step 5 token migration a single shared edit.
- **No third-party tracking, analytics, or social-share scripts** — the site is privacy-respecting by default (a real positive for a HIPAA brand).
- **`transition: max-height` on FAQ accordion uses opacity + height correctly** (not animating layout in a way that thrashes).

---

## Recommended Actions (priority order)

1. **[P0] `$impeccable harden`** — Add a global `:focus-visible` rule, add `prefers-reduced-motion` gates around all keyframes and counter/scroll-reveal JS, fix heading hierarchy by promoting industry/case-study/FAQ titles to `<h3>`.
2. **[P0] `$impeccable distill`** — Remove the hero-metric stats template, the gradient-shift top-bar, the pulseSoon/pulseAnswer/phonePulse decorative animations, and the gradient-text on hero accent. Strip-back rather than replace.
3. **[P0] `$impeccable typeset`** — Resolve the Cabinet-Grotesk-not-imported bug (replace with Cormorant Garamond + Instrument Sans + Space Grotesk per DESIGN.md). This is also Step 5 of the multi-step plan; do it once, in that step.
4. **[P0] `$impeccable layout`** — Rewrite `.audio-demo__callout-box`, `.comparison__card--without`, `.comparison__card--with` away from side-stripe-border patterns to full-border + tinted-background per DESIGN.md.
5. **[P1] `$impeccable clarify`** — Replace every "Get Started → / Read More → / Learn More →" with action-and-outcome copy. Fix the 97 em dashes. Replace bare `href="#"` placeholders with real targets or `<button>` semantics.
6. **[P1] `$impeccable adapt`** — Increase nav CTA and mobile menu touch targets to clear 44×44. Add `viewport-fit=cover` and safe-area-inset support.
7. **[P1] `$impeccable colorize`** — Replace pure-white page surfaces with ivory; replace pure-black-on-yellow badges with charcoal-on-tinted-yellow; ensure body text color reaches 4.5:1 on the new ivory background.
8. **[P1] `$impeccable bolder`** — Vary the case-study card grid so it reads as three different stories, not three slots in a template.
9. **[P2] `$impeccable optimize`** — Add `width`/`height` to logo, switch `transition: all` to explicit-property + ease-out-quart, fix FAQ max-height clipping.
10. **[P2] `$impeccable polish`** — After all fixes land: re-run audit for systemic-issue verification (token sprawl resolved, no slop tells, scoring 14+/20).

> You can ask me to run these one at a time, all at once, or in any order you prefer. Re-run `$impeccable audit` after fixes to see your score improve.

Recommended sequence given the multi-step plan: **distill → typeset → layout → clarify → harden → adapt → colorize → bolder → optimize → polish**. The first three resolve P0 anti-pattern violations and the typography migration in one go; the rest layer on conversion + accessibility wins. Critique (`$impeccable critique`, Step 3 of the plan) is the right *next* user-driven step to score the design dimension; this audit only covers technical quality.
