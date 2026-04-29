# Sera — Design Critique

**Date:** 2026-04-28
**Method:** Two isolated assessments — Agent-A LLM design review + Agent-B deterministic CLI scan (`npx impeccable@2.1.8`) — synthesized.
**Standard:** Nielsen 10 heuristics, cognitive-load checklist, persona walkthroughs, AI-slop test.

---

## Design Health Score

| # | Heuristic | Score | Key Issue |
|---|-----------|-------|-----------|
| 1 | Visibility of System Status | 3 / 4 | Audio player + scroll-progress give feedback. CTAs anchor to `#dark-cta`; on pages without that anchor (e.g., pricing.html:944) the click silently fails. |
| 2 | Match System / Real World | 3 / 4 | Outcome-led copy mostly. Regression: pricing hero says "Transparent, Coverage-Based Pricing" — "coverage-based" is internal product language, not how a practice owner thinks. |
| 3 | User Control and Freedom | 2 / 4 | Mobile menu has no close button (only closes on link tap). FAQ single-open behavior removes ability to compare two answers. No keyboard Esc on mobile menu. |
| 4 | Consistency and Standards | **1 / 4** | **Brand color is literally not the same across pages.** pricing.html / product.html / solutions.html each override `--green: #16A34A` at line 22; index.html and css/styles.css carry `--green: #0D9B6A`. Same company, two different brand greens depending on which page you load. |
| 5 | Error Prevention | 2 / 4 | "Get Started" CTAs on pricing.html:1004, 1046 link to `#`. "Contact Sales" at 1086 links to `#`. The primary revenue-conversion path dead-ends. No form validation anywhere (no forms exist in current markup). |
| 6 | Recognition Rather Than Recall | 3 / 4 | Persistent labeled nav, working active states. Long product.html page (2772 lines) lacks an in-page section nav for power users. |
| 7 | Flexibility and Efficiency | 2 / 4 | Live phone CTA `tel:3476531170` (index.html:529) is the one strong power-user shortcut. No keyboard shortcuts elsewhere. No "jump to pricing" fast path. |
| 8 | Aesthetic and Minimalist Design | **1 / 4** | The hero stacks ~9 simultaneous animated elements above the fold (badge pulse, gradient headline, 3D card stack with continuous float, hero blob, dot grid, two pulse dots, waveform bars, scroll-progress gradient). The brand claims "calm" — the demonstration is restless. |
| 9 | Error Recovery | 1 / 4 | Dead CTAs offer no recovery path. No 404 referenced. No inline error patterns found. Mobile menu has no visual close affordance. |
| 10 | Help and Documentation | 2 / 4 | FAQ on pricing + security is substantive. Security page is genuinely credible. Missing: no onboarding context on demo flow ("what happens after I book"), no inline help on pricing tier differences. |
| **Total** | | **20 / 40** | **Acceptable (low end) — significant work needed before users are happy** |

**Rating bands**: 36-40 Excellent · 28-35 Good · 20-27 Acceptable · 12-19 Poor · 0-11 Critical. Sera sits at the bottom of Acceptable, one heuristic away from Poor.

---

## Anti-Patterns Verdict

**LLM assessment (Agent A):** **FAIL — looks AI-generated.** Specific bans triggered:

1. **Gradient text** (absolute ban) — `index.html:50` `.hero__headline .green` (linear-gradient on the hero's most important word) and `index.html:163` `.stat__number` (gradient on every stat number). Agent A independently flagged this; matches DESIGN.md §6 Don'ts.
2. **Side-stripe borders** (absolute ban) — `index.html:154` `.audio-demo__callout-box { border-left: 4px solid var(--green) }` and the comparison-card pair at lines 383-384. Textbook left-stripe accent.
3. **Top animated gradient bar** — `pricing.html:66-82`, `solutions.html:73-88`, `product.html:87-98` open with a 3px animating green-to-teal gradient pinned to the viewport top. The signature generic-SaaS landing tell.
4. **Hero-metric template** — `index.html:606-609` four stat-cards in an even grid, gradient-fill numbers, secondary labels. Identical structure to a thousand other AI-tool landings.
5. **Pure-white page backgrounds** — `pricing.html:59`, `solutions.html:52`, `product.html:56` all use `background: #fff` on the page surface. Violates DESIGN.md's No-Pure rule.
6. **Pill buttons everywhere** — `css/styles.css:117` sets `border-radius: var(--radius-pill)` on every `.btn`. The register reads consumer-friendly, not operator-serious. DESIGN.md mandates 4px sharp rectangles.
7. **Emoji as industry icons** — `solutions.html` uses `🦴` (bone) for PT, `🦷` (tooth) for Dental, `✨` for Med Spa, `🔧` for Chiro. In a healthcare B2B context, emoji-as-icon undermines the surgical / clinical register entirely.
8. **Category-reflex green** — The brand uses Tailwind green-600 (`#16A34A`) on three pages, the system green (`#0D9B6A`) on the others. Both read as "category-reflex healthcare green." Neither is the muted, document-grade `#2d7d5f` DESIGN.md mandates.

**Deterministic scan (Agent B):** 132 findings across 5 pages. Grouped:

| Antipattern | Count | Notes |
|---|---|---|
| **low-contrast** | 59 | Largely white text on dark gradient backgrounds the detector cannot trace through layered backgrounds — most are likely false positives. **However** a non-trivial subset are real: white-on-yellow `.flow-tab__soon` badges, white text on gradient backgrounds with mid-stops below contrast threshold. Worth re-running once page surfaces migrate to ivory. |
| **tiny-text** | 54 | Body text < 12px. Concentrated in `product.html` (36) and `index.html` (12). Confirms the hero-card sub-label issue Agent A flagged (0.55-0.7rem text inside the floating card stack). Real failure for mobile readability. |
| **side-tab** | 6 | All in `index.html` — likely the call-flow scenario tabs. The side-tab pattern is on the edge of the side-stripe ban; treat as moderate. |
| **overused-font** | 5 | One per page — Inter is used for body. Replacing with Instrument Sans (per DESIGN.md) resolves the tell while keeping the same readability profile. |
| **layout-transition** | 5 | One per page — `transition: width` (scroll-progress) and `transition: max-height` (FAQ accordion). Both produce layout thrash. Switch to `transform: scaleX` (progress) and `grid-template-rows` (accordion) per the parent skill's motion law. |
| **gradient-text** | 1 | Detector caught only the hero accent, missed `.stat__number`. Agent A caught both. |
| **bounce-easing** | 1 | One overshooting `cubic-bezier` somewhere in `index.html`. Probably the card-stack reveal. |
| **dark-glow** | 1 | The unused `--shadow-glow` token (`0 0 60px var(--green-glow)`). Dead code. |

**Where the two assessments converge:** Both flag (1) gradient text on hero, (2) Inter as overused, (3) layout-property transitions, (4) tiny text in hero card, (5) the overall AI-slop verdict. The agreement on independent methodologies is strong evidence — these are not opinion calls.

**Where they diverge:** The CLI's 59 low-contrast findings are inflated (it can't trace gradient backgrounds), so the *real* number of contrast failures is smaller — perhaps 10-15 — but those are still WCAG AA violations. The LLM did not catch the layout-transition issue because it requires inspecting the easing/property combination rather than reading copy.

---

## Overall Impression

**The site demonstrates the wrong brand.** PRODUCT.md's three brand words are *trustworthy, surgical, calm*. The current implementation reads as *busy, motivated, friendly*. That's not a tonal disagreement at the surface — it's the foundation. Until that's fixed, every section-level refinement is putting frosting on the wrong cake.

**The biggest opportunity is subtractive, not additive.** The site has the right *information* (security page is genuinely credible, audio demo concept is right, phone-number CTA is differentiated). What it doesn't have is restraint. The strongest move is removing — the gradient bar, the gradient text, the floating 3D hero, the pulse animations, the emoji icons, the second `--green` token, the dead-link CTAs — and letting the actual product proof (audio demo, security architecture, named clinic numbers if real) carry the page.

The audio demo section is the page's strongest moment. If a practice owner clicks "Try It Now: (347) 653-1170" and hears the AI work, the conversion is essentially done. **The site's job is to get them to that click without any AI-tool tells in their way.** Right now the visual language is doing the opposite of that job.

---

## What's Working

1. **The phone-number CTA is genuinely differentiated.** `index.html:529` "Try It Now: (347) 653-1170" — a tappable live phone number to experience the product directly, above the fold, on every page. This is the single strongest trust signal on the site and the right answer to the practice owner's "does this actually work?" doubt.

2. **The audio-demo section concept is sound.** `index.html:557-599` is a full-section showcase: tabbed demos by specialty (PT, Dental, Chiropractor), an audio player, and a side-by-side live-summary card showing what the front desk receives. The information architecture (hear the call, watch the summary build) maps directly to the practice owner's job-to-be-done. Visual treatment needs work; the structure is right.

3. **The security page has substance.** `security.html:288-366` lays out the HIPAA framework as Administrative / Physical / Technical safeguards with specific line items (AES-256, TLS 1.3, MFA, session timeout). This is the level of specificity a practice owner evaluating compliance needs. Most healthcare-SaaS sites stop at a "HIPAA-Compliant" badge; Sera goes further. Preserve this.

4. **Outcome-focused base copy.** Hero subhead, trust pills ("Answers Calls Instantly," "Books Into Your System"), and stat captions ("Reclaimed for patient care") all lead with outcome, not feature. The copy DNA is right; the visual treatment fights it.

---

## Priority Issues (Top 5)

### **[P0] Brand color is literally inconsistent across pages**
- **What:** `pricing.html`, `product.html`, `solutions.html` redeclare `--green: #16A34A` at line 22; `index.html` and `css/styles.css` use `#0D9B6A`. A user navigating from index → pricing → product sees three different shades of "Sera green."
- **Why it matters:** Brand consistency is the load-bearing trust signal of a healthcare SaaS. A buyer who notices the brand doesn't agree with itself reads the company as scrappy / unfinished. Conversion penalty is invisible but real.
- **Fix:** Delete the local `:root` overrides on the three pages. Migrate `css/styles.css` to the target `--healthcare-green: #2d7d5f` per DESIGN.md and let it cascade. This is one of the things Step 5 (token migration) was always going to fix; flagging here so it isn't deferred.
- **Suggested command:** `$impeccable harden`

### **[P0] Dead CTAs across the conversion path**
- **What:** Every "Get Started →" on pricing.html (lines 1004, 1046), "Contact Sales →" (1086), every "Read More →" on case-study cards (index.html:865-875), every "Learn More →" on solutions industry cards (1080, 1121, 1162, 1203) links to bare `href="#"`. The primary revenue-conversion path dead-ends.
- **Why it matters:** This is not a design issue — it is a broken funnel. A prospect who reaches pricing, decides on a tier, clicks the CTA, and lands at the top of the same page will not retry; they will leave.
- **Fix:** Wire each CTA to a real target — Calendly embed for "Book Demo," `mailto:` or `/contact` for "Talk to Sales," anchor to `#pricing` from "Start Trial," anchor to a case-study page or `index.html#case-studies` for "Read the Case Study." Replace placeholders with real destinations OR remove the CTAs until destinations exist.
- **Suggested command:** `$impeccable clarify` (copy + destination together)

### **[P0] The hero is doing nine things at once**
- **What:** Above the fold on `index.html` simultaneously: animated badge with pulsing dot, gradient-text headline, sub-headline, two CTAs, four trust pills, social-proof row of five clinic names, floating 3D card stack with `perspective` transform and continuous float animation, two pulse-dot animations, hero blob radial gradient, and a dot-grid background. Cognitive load checklist: 6 of 8 items fail.
- **Why it matters:** PRODUCT.md's three brand words include *calm*. The current implementation demonstrates the opposite. A practice owner evaluating in 10 seconds reads "another AI tool" and bounces.
- **Fix:** Remove the hero blob, the dot-grid background, the 3D perspective on the card stack, and the continuous float. Let the card be static. Keep one pulse — the live-status dot on the card header. Remove the social-proof row (or move it 1 scroll down — five clinic names without quotes is noise here). Trim from nine moving parts to three: headline, two CTAs (with phone CTA promoted to primary), one static visual proof (the call-summary card).
- **Suggested command:** `$impeccable quieter` then `$impeccable distill`

### **[P1] Gradient text + pill buttons signal the wrong register**
- **What:** Hero accent gradient (`index.html:50`) and stat numbers (`:163`) use `background-clip: text`. All buttons use `--radius-pill` (100px). The visual language is consumer-friendly SaaS / AI tool.
- **Why it matters:** PRODUCT.md anti-references explicitly call these out: "purple-to-pink gradients … rounded-pill buttons for primary actions." Pattern-matching wins or loses the buyer in the first 3 seconds.
- **Fix:** Replace gradient text with charcoal solid; emphasis through scale/weight, not gradient. Replace pill radius with 4px sharp rectangles per DESIGN.md §5. (Chips remain pill — they're a different component.)
- **Suggested command:** `$impeccable typeset` (gradient → solid) + `$impeccable bolder` (button shape register)

### **[P1] Unverified claims undermine the trust they're meant to build**
- **What:** "0 Data Breaches — Ever" (security.html:260-278), "Trusted by 50+ healthcare practices" (index.html:536), "15-20 hrs Saved Weekly" (index.html:608), "Save $35K+/year vs. hiring a full-time receptionist" (index.html:909). None link to evidence — no SOC 2 download, no named case study, no methodology, no clinic owner attribution. Riley and Dana both flag these.
- **Why it matters:** PRODUCT.md principle 2: "Trust through specificity." Generic credibility signals are noise; specific ones are signal. Asserting "$35K saved" without "at Summit Physical Therapy in Boulder, in 90 days, recovering 41 missed appointments at $850 ACV" is the same shape as a fabricated number — and the buyer treats it that way.
- **Fix:** Pick **one** named clinic, one real number, one verifiable timeframe. Make that the spine of the social proof. Replace the "Trusted by" logo line with a pull-quote that names the practice owner, city, and metric. If the named-clinic data doesn't exist yet, *say* "early access — pilot data available on request" rather than asserting numbers.
- **Suggested command:** `$impeccable clarify`

---

## Persona Red Flags

**Jordan (Confused First-Timer)** — practice owner who has never seen an AI receptionist tool

1. `index.html:527` headline assumes Jordan knows the category. "Never Miss Another Patient's Call Again" answers *what problem*, not *what solution*. They don't know if this is a virtual answering service, a chatbot, an IVR, or AI.
2. `index.html:665` "Outbound Calls — Soon" badge in the product's own scenario-handling section signals incomplete product. Jordan logs as risk.
3. `pricing.html:953` headline "Coverage-Based Pricing" — Jordan doesn't know what "coverage-based" means and won't stop to read the explanation. They'll skip to the price numbers and feel mildly disoriented.
4. `solutions.html:1080` "Learn More →" linking to `#`. First click breaks trust.
5. `index.html:563-565` audio-demo tabs labeled "Dental Office," "PT Clinic," "Chiropractor" without context. Jordan doesn't know what they're about to hear.

**Casey (Distracted Mobile User)** — operator scrolling between patients on a phone

1. `index.html:41-43` hero has three stacked background layers (two radial gradients + dot grid) plus the blob plus the card stack. On a mid-tier Android, the above-the-fold paint is slow. Casey bounces before content loads.
2. `index.html:549` the `0.7rem` text inside the hero card (tiny-text antipattern, 12 instances on this page) is illegible at mobile sizes. Per PRODUCT.md, body text should not go below 17-18px on mobile.
3. `css/styles.css:349-363` `.nav__cta` has `0.55rem 1.5rem` padding — ~31px tall. Sub-44px touch target. WCAG 2.5.5 violation. The primary mobile CTA cannot be reliably tapped.
4. The nav CTA disappears on mobile entirely (collapsed into the hamburger). Casey's "Book a Demo" path requires opening the menu, scrolling, tapping. Three actions where one should suffice.
5. `index.html:629` integration marquee logos rendered as inline `<span>` tags with inline color styles. Not tappable, not labeled. Casey can't tell if these are partners, clients, or integrations.

**Riley (Deliberate Stress Tester)** — buyer who probes claims and tries to break the conversion path

1. `pricing.html:1086` Enterprise CTA "Contact Sales →" → `#`. Riley clicks to test sales path. Dead.
2. `security.html:260-278` "0 Data Breaches — Ever." Riley wants verification. No SOC 2 link, no audit report, no BAA template. Unverifiable claim.
3. `index.html:536` "Trusted by 50+ healthcare practices" + five named clinics. Riley searches one — real-sounding but unverifiable. No case study, no quote, no logo, no link.
4. `index.html:584` audio player labeled "Real AI conversation · Not scripted · Not edited." Sample HTML showed no `<audio src>` — if there's no actual audio file wired, the demo section is visual-only theater.
5. `pricing.html:1004,1046` primary tier CTAs all link to `#`. The revenue path is broken.

**Dana (PT Practice Owner, 50)** — busy, skeptical of AI hype, tracks five vendors at once

1. `index.html:606-609` stats — "0 Missed Calls While Live," "15-20 hrs Saved Weekly." Live for whom? For how long? Verified by? Dana has been burned. Unattributed numbers read as boilerplate.
2. `index.html:529` primary CTA is "Book a Demo →"; secondary is the phone number. Dana wants pricing first. Path: hero → nav → pricing → no working CTA → back. Three extra steps before she can convert.
3. `solutions.html:1073` "Reduce front-desk workload by 15-20 hours per week." Unattributed. Dana needs a named clinic, time period, methodology.
4. `pricing.html:1009` featured tier "Recommended · Best Value" — "best value" compared to what? Dana wants the math vs. a human receptionist OR an answering service. The comparison table exists lower on the page but isn't anchored from the tier.
5. `index.html:909` "Save $35K+/year vs. hiring a full-time receptionist." Dana asks: which receptionist, in which market, at what loaded cost? Unsourced.

---

## Cognitive Load Assessment

**Failures: 6 of 8 — Critical**

| Item | Pass / Fail | Why |
|---|---|---|
| Single focus | **Fail** | Hero presents 9 simultaneous focal points above the fold. |
| Chunking | **Fail** | index.html flat-scrolls 17 named sections. No grouping, no progressive reveal. |
| Grouping | Pass | Sections delineated with background alternation. |
| Visual hierarchy | **Fail** | All section headlines are weight-800 Cabinet Grotesk (which silently falls back to system-ui). Display/Headline/Title scale ratios are flat. |
| One-thing-at-a-time | Pass | how-it-works section sequences correctly. |
| Minimal choices | **Fail** | solutions.html:1060 and security.html:239 each present two equal-weight CTAs. PRODUCT.md principle 4: "One primary action per surface." |
| Working memory | **Fail** | Pricing tier feature lists require holding 8 Starter items in memory while reading 7 Pro items. The comparison table that resolves this is not anchored from the tiers. |
| Progressive disclosure | **Fail** | All content is immediately visible. FAQ accordion is the only example. No collapsible details, no "show more," no tabbed feature drill-down. |

---

## Minor Observations

- `js/main.js:42-51` FAQ single-open removes user agency — a buyer comparing two related answers can't open both. Allow multiple-open.
- `css/styles.css:579` `max-height: 500px` on open FAQ answer will clip any answer longer than ~500px. Switch to `grid-template-rows: 0fr → 1fr` transition for content-aware sizing.
- The favicon (`favicon.svg`) exists; consider an additional `<link rel="icon" media="(prefers-color-scheme: dark)" …>` for dark-mode browsers.
- Open Graph + Twitter Card meta missing on most pages — social shares will pick first paragraph as preview.
- No structured data (`<script type="application/ld+json">`) — adding `Organization` + `Product` schema may help healthcare-SaaS SEO.
- The `--shadow-glow` token (`0 0 60px var(--green-glow)`) is defined but unused. Dead code; the CLI flagged it as dark-glow. Delete.

---

## Questions to Consider

**1. If the phone-number CTA is the only proof the product is real, why is it the secondary button?** "Book a Demo →" is the primary; "Try It Now: (347) 653-1170" is the outline. For a product whose value proposition is *answering calls*, the phone number arguably should be the primary CTA — it proves the product before the user has to commit to a calendar slot. Would flipping the hierarchy reduce drop-off between hero and conversion?

**2. What would the homepage look like if you removed every element added to "look credible" and kept only what demonstrates the product?** The hero's nine elements collapse to two: the headline and the call-summary card. The trust-pill row, the "trusted by 50+" line, the gradient bar, the social-proof clinic names — every one of those is *asserting* trust rather than *demonstrating* it. The audio demo and the security page architecture demonstrate it. What if the entire site was structured around the demonstration-vs-assertion axis, and asserted only what was already demonstrated?

**3. The brand words are "trustworthy, surgical, calm." The site reads as "busy, motivated, friendly." Which is the real brand — the stated one or the demonstrated one?** This is the load-bearing question. If the answer is "the stated one," then ~70% of the homepage's animated, decorative, friendly-rounded elements are debt to be paid down. If the answer is "the demonstrated one," then PRODUCT.md needs an honest rewrite. The two cannot both be true.
