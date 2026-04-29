---
name: Sera
description: An AI receptionist for healthcare practices — designed for the operator who answers to no one but their patients.
colors:
  charcoal: "#1a1a1a"
  charcoal-soft: "#2a2a2a"
  ink: "#3a3a3a"
  graphite: "#5a5a5a"
  slate: "#7a7a7a"
  rule: "#e6e3da"
  paper: "#f5f2ea"
  ivory: "#fbf9f3"
  pure-white: "#ffffff"
  healthcare-green: "#2d7d5f"
  healthcare-green-deep: "#225f48"
  healthcare-green-tint: "#e9f1ec"
  clinical-blue: "#0066cc"
  clinical-blue-deep: "#0050a3"
  clinical-blue-tint: "#e6f0fa"
  terracotta: "#c85a2a"
  terracotta-deep: "#a3471f"
  terracotta-tint: "#f6e8e0"
  signal-positive: "#2d7d5f"
  signal-warning: "#c89028"
  signal-danger: "#a83838"
typography:
  display:
    fontFamily: "Cormorant Garamond, Georgia, 'Times New Roman', serif"
    fontSize: "clamp(2.5rem, 6vw, 4.25rem)"
    fontWeight: 500
    lineHeight: 1.05
    letterSpacing: "-0.02em"
  headline:
    fontFamily: "Cormorant Garamond, Georgia, serif"
    fontSize: "clamp(1.875rem, 3.6vw, 2.75rem)"
    fontWeight: 500
    lineHeight: 1.1
    letterSpacing: "-0.015em"
  title:
    fontFamily: "Instrument Sans, system-ui, sans-serif"
    fontSize: "1.25rem"
    fontWeight: 600
    lineHeight: 1.3
    letterSpacing: "-0.005em"
  body:
    fontFamily: "Instrument Sans, system-ui, sans-serif"
    fontSize: "1.0625rem"
    fontWeight: 400
    lineHeight: 1.6
    letterSpacing: "0"
  body-small:
    fontFamily: "Instrument Sans, system-ui, sans-serif"
    fontSize: "0.9375rem"
    fontWeight: 400
    lineHeight: 1.55
    letterSpacing: "0"
  label:
    fontFamily: "Space Grotesk, 'IBM Plex Sans', sans-serif"
    fontSize: "0.75rem"
    fontWeight: 600
    lineHeight: 1.2
    letterSpacing: "0.12em"
  data:
    fontFamily: "Space Grotesk, 'IBM Plex Mono', monospace"
    fontSize: "1rem"
    fontWeight: 500
    lineHeight: 1.4
    letterSpacing: "0"
  metric:
    fontFamily: "Space Grotesk, sans-serif"
    fontSize: "clamp(2rem, 4vw, 3rem)"
    fontWeight: 600
    lineHeight: 1
    letterSpacing: "-0.01em"
rounded:
  none: "0px"
  sm: "4px"
  md: "8px"
  lg: "12px"
  pill: "999px"
spacing:
  xs: "4px"
  sm: "8px"
  md: "16px"
  lg: "24px"
  xl: "40px"
  xxl: "64px"
  section: "104px"
components:
  button-primary:
    backgroundColor: "{colors.charcoal}"
    textColor: "{colors.ivory}"
    rounded: "{rounded.sm}"
    padding: "14px 26px"
  button-primary-hover:
    backgroundColor: "{colors.healthcare-green}"
    textColor: "{colors.ivory}"
    rounded: "{rounded.sm}"
    padding: "14px 26px"
  button-secondary:
    backgroundColor: "{colors.ivory}"
    textColor: "{colors.charcoal}"
    rounded: "{rounded.sm}"
    padding: "14px 26px"
  button-secondary-hover:
    backgroundColor: "{colors.paper}"
    textColor: "{colors.charcoal}"
    rounded: "{rounded.sm}"
    padding: "14px 26px"
  button-ghost-dark:
    backgroundColor: "{colors.charcoal}"
    textColor: "{colors.ivory}"
    rounded: "{rounded.sm}"
    padding: "14px 26px"
  card:
    backgroundColor: "{colors.ivory}"
    textColor: "{colors.charcoal}"
    rounded: "{rounded.md}"
    padding: "32px"
  card-on-paper:
    backgroundColor: "{colors.pure-white}"
    textColor: "{colors.charcoal}"
    rounded: "{rounded.md}"
    padding: "32px"
  chip-default:
    backgroundColor: "{colors.paper}"
    textColor: "{colors.ink}"
    rounded: "{rounded.pill}"
    padding: "6px 12px"
  chip-green:
    backgroundColor: "{colors.healthcare-green-tint}"
    textColor: "{colors.healthcare-green-deep}"
    rounded: "{rounded.pill}"
    padding: "6px 12px"
  input-default:
    backgroundColor: "{colors.pure-white}"
    textColor: "{colors.charcoal}"
    rounded: "{rounded.sm}"
    padding: "12px 14px"
  input-focus:
    backgroundColor: "{colors.pure-white}"
    textColor: "{colors.charcoal}"
    rounded: "{rounded.sm}"
    padding: "12px 14px"
  pricing-tier-default:
    backgroundColor: "{colors.ivory}"
    textColor: "{colors.charcoal}"
    rounded: "{rounded.md}"
    padding: "40px 32px"
  pricing-tier-featured:
    backgroundColor: "{colors.charcoal}"
    textColor: "{colors.ivory}"
    rounded: "{rounded.md}"
    padding: "40px 32px"
  metric-block:
    backgroundColor: "{colors.paper}"
    textColor: "{colors.charcoal}"
    rounded: "{rounded.md}"
    padding: "24px 28px"
---

# Design System: Sera

## 1. Overview: The Operator's Console

**Creative North Star: "The Operator's Console"**

Sera's surface reads like a console for someone who runs a business. A serious, calm, document-grade interface — closer to Stripe's restraint or a medical journal's gravity than to consumer SaaS. Trust is earned through precision: tight type, generous whitespace, restrained color, surgical use of green. The site addresses an operator at 7am between patients, scrolling on a phone, deciding whether Sera is real. Every element is an answer to a doubt; nothing is decoration.

The system is Restrained on the color-strategy axis: tinted neutrals carry the surface, with the healthcare green appearing on roughly 10% of any given screen. That rarity is the point — when green appears, it carries weight. The Cormorant Garamond display face supplies the editorial-medical seriousness; Instrument Sans does the operational reading; Space Grotesk handles the numbers and labels with a quiet, technical confidence. Surfaces are flat at rest; shadows appear only as a response to state.

We explicitly reject the visual language of generic SaaS landing pages and AI-tool marketing. No bento grids of identical icon-and-paragraph cards, no purple gradients or glow orbs, no "powered by GPT" badges, no smiling-receptionist-in-scrubs stock photography. The category-reflex pull toward "healthcare = teal-on-white-medical" is forbidden — we go warmer, darker, and quieter.

**Key Characteristics:**
- Charcoal-dominant surface with cream/ivory neutrals, never pure white
- One accent (healthcare green) used on ≤10% of any view
- Editorial serif display + operational sans + technical mono
- Flat by default; depth from layering and rule lines, not shadows
- Type scale ratio ≥ 1.4 between major steps (declarative hierarchy)
- Zero ornament — every glyph, line, and shape earns its place

## 2. Colors: The Console Palette

A muted, document-paper field with one quiet green accent, one rare blue for data, and one warmer color reserved for human moments. Pure white and pure black are forbidden — every neutral is tinted toward warm gray.

### Primary
- **Console Charcoal** (`#1a1a1a`): The dominant ink. Sets headlines, body copy, primary buttons, the dark hero, and the final CTA section. Carries the sense of authority and document-permanence.
- **Soft Charcoal** (`#2a2a2a`): A second-order ink for slightly less aggressive headlines or dark surfaces over a charcoal hero.

### Secondary
- **Healthcare Green** (`#2d7d5f`): The single accent. Reserved for primary CTA hover states, link emphasis, healthcare-trust chips, positive metrics, and the brand mark. Never used as a flood color across a whole section.
- **Healthcare Green Deep** (`#225f48`): For green-on-green hover or pressed states; for green text on green-tinted backgrounds where contrast is required.
- **Healthcare Green Tint** (`#e9f1ec`): A whisper-tint for chips, badges, the HIPAA pill in the footer, success-state backgrounds.

### Tertiary
- **Clinical Blue** (`#0066cc`): Reserved for data — chart strokes, link underlines on long-form pages, "Read the [security paper]" anchors, anything that signals factual reliability. Never used as a CTA color.
- **Clinical Blue Tint** (`#e6f0fa`): Background for data callouts, code samples, security feature blocks.
- **Terracotta** (`#c85a2a`): The single warm color. Used in fewer than 1% of cases — testimonial signature lines, the founder's note, the human-handoff illustration. Never used for CTAs, navigation, or anything operational.

### Neutral
- **Ivory** (`#fbf9f3`): The dominant page background — a warm-tinted off-white. Pure white is forbidden as a page background.
- **Paper** (`#f5f2ea`): A second-order surface for cards-on-ivory and quiet sections that need a subtle layer change.
- **Pure White** (`#ffffff`): Reserved for cards-on-paper, input fields, and the inside of dialog surfaces. Never as a top-level page background.
- **Rule** (`#e6e3da`): A single warm hairline used for all dividers, card borders, and table lines.
- **Graphite** (`#5a5a5a`): Secondary body text and axis labels.
- **Slate** (`#7a7a7a`): Tertiary text — captions, footnotes, time-ago labels.

### Named Rules

**The 10% Green Rule.** Healthcare Green appears on no more than 10% of any rendered viewport. Its rarity is the load-bearing trust signal. If the green count rises above that line, redesign — do not dilute the brand by spreading it thin.

**The Terracotta Whisper Rule.** Terracotta exists only for moments of human warmth: a testimonial signature, an "in their own words" pull-quote, a single underline on a clinic owner's name. Never use it for buttons, badges, navigation, status, or charts.

**The No-Pure Rule.** Neither `#000` nor `#fff` is allowed in production CSS. The palette is tinted at every step. Pure values read as default; we are not default.

**The Category-Reflex Ban.** No teal, no medical-aqua, no clinic-cyan, no white coats. Healthcare-by-cliché is forbidden; healthcare-by-restraint is the brand.

## 3. Typography

**Display Font:** Cormorant Garamond (with Georgia, 'Times New Roman', serif fallbacks)
**Body Font:** Instrument Sans (with system-ui, sans-serif fallbacks)
**Data / Label Font:** Space Grotesk (with 'IBM Plex Mono', monospace fallbacks for numerics)

**Character:** A medical-journal serif paired with a technical sans, joined by a numerically confident grotesk. Cormorant supplies the gravity; Instrument supplies the operational rhythm; Space Grotesk supplies the numeric authority. Together they read as "a serious operator's manual," not "a SaaS landing page."

### Hierarchy

- **Display** (Cormorant Garamond, 500, `clamp(2.5rem, 6vw, 4.25rem)`, line-height 1.05, letter-spacing -0.02em): Hero headlines and the final CTA headline only. One per page.
- **Headline** (Cormorant Garamond, 500, `clamp(1.875rem, 3.6vw, 2.75rem)`, line-height 1.1): Section headlines on standard pages, pricing tier names, case-study titles.
- **Title** (Instrument Sans, 600, 1.25rem, line-height 1.3): Card titles, feature subheads, FAQ questions, table column headers.
- **Body** (Instrument Sans, 400, 1.0625rem, line-height 1.6): Paragraph text. Capped at 65–75 characters per line for paragraph blocks.
- **Body Small** (Instrument Sans, 400, 0.9375rem, line-height 1.55): Card body text, captions in feature blocks.
- **Label** (Space Grotesk, 600, 0.75rem, line-height 1.2, letter-spacing 0.12em, ALL CAPS): Eyebrow labels above section headlines, button labels, status chips.
- **Data** (Space Grotesk, 500, 1rem, line-height 1.4): Tabular figures, integration code identifiers, technical specs in security copy.
- **Metric** (Space Grotesk, 600, `clamp(2rem, 4vw, 3rem)`, line-height 1, letter-spacing -0.01em): Standalone statistics — "$42K," "87%," "3.2 sec." Always paired with a Body Small caption beneath.

### Named Rules

**The One Display Rule.** Display type appears once per page maximum — usually the hero. A second occurrence steals weight from the first and breaks hierarchy.

**The Numeric Authority Rule.** Numbers always render in Space Grotesk, never Cormorant or Instrument. A clinic owner's eye must catch metrics as a category distinct from prose.

**The 65-Char Rule.** Body paragraphs are constrained to 65–75ch. Wider lines reduce comprehension by ~15% in eye-tracking studies; healthcare buyers re-read, and re-reading benefits from a tight measure.

**The Cap-Lock Eyebrow Rule.** Eyebrow labels are ALL CAPS in Space Grotesk with 0.12em tracking. They mark a section type without competing with the headline beneath.

## 4. Elevation

This system is flat by default. Surfaces sit at rest with no shadow; depth is conveyed through tonal layering (Ivory → Paper → Pure White) and a single warm hairline (`#e6e3da`). Shadows appear only as a response to interactive state — hover, focus, drag — or to lift truly elevated objects (the sticky nav after scroll, an open dialog).

### Shadow Vocabulary

- **state-hover** (`box-shadow: 0 1px 2px rgba(26,26,26,0.04), 0 8px 24px rgba(26,26,26,0.06)`): Cards on hover, pricing tiers on hover. Tinted toward charcoal, never toward black.
- **state-focus** (`box-shadow: 0 0 0 3px rgba(45,125,95,0.20)`): Focus ring on inputs and keyboard-focused interactive elements. Healthcare-green at 20% — the only place green appears as a glow.
- **lift-nav** (`box-shadow: 0 1px 0 rgba(26,26,26,0.06)`): A single hairline appears under the sticky nav once the user has scrolled past the hero. Not a shadow in the conventional sense — a rule line that signals layer change.
- **lift-dialog** (`box-shadow: 0 24px 64px rgba(26,26,26,0.18), 0 4px 12px rgba(26,26,26,0.08)`): For modal dialogs and popovers only. Never on a card.

### Named Rules

**The Flat-By-Default Rule.** No surface carries a shadow at rest. If a card "needs a shadow to look like a card," its border, padding, or surface tint is wrong — fix that, don't add a shadow.

**The Charcoal-Tinted Shadow Rule.** Every shadow uses `rgba(26,26,26, x)` — the charcoal token at low opacity — never `rgba(0,0,0, x)`. Pure-black shadows on a tinted surface read as artificial.

**The Single-Hairline Rule.** Borders and dividers use one and only one color (`#e6e3da`) at one and only one weight (1px). No double-rules, no inset highlights, no border-bottom-then-border-top tricks.

## 5. Components

### Buttons

- **Shape:** Sharp rectangles with a 4px radius (Rounded `sm`). No pill buttons except for chips.
- **Primary:** Charcoal background, ivory text, 14×26 padding, label in Instrument Sans 600 0.95rem with letter-spacing -0.005em. On hover, the background shifts to Healthcare Green (the system's only flood-of-green moment) and lifts 1px via translateY. Transition: 150ms ease-out-quart.
- **Secondary:** Ivory background, charcoal text, 1px Rule border. Hover: background shifts to Paper, no translate.
- **Ghost on dark:** Transparent background, ivory text, 1px ivory-at-25% border. For the dark hero / final CTA section. Hover: border solidifies, background fills to ivory-at-8%.
- **Phone-link button:** Phone-number CTAs (`tel:`) render as Secondary buttons with a Space Grotesk numeric label so the digits read as data, not prose.
- **Forbidden:** Rounded-pill buttons for primary actions, gradient backgrounds, glow on rest, drop shadow on rest, "Get Started" or "Learn More" labels.

### Chips

- **Default:** Paper background, Ink text (1px Rule border optional), 6×12 padding, Label type at 0.7rem ALL CAPS Space Grotesk.
- **Healthcare-Green chip:** Healthcare-Green-Tint background, Healthcare-Green-Deep text. For HIPAA badges, "Live now" indicators, positive trust signals.
- **State:** Chips are static signals, not interactive. If a chip would benefit from being clickable, it should become a Secondary button instead.

### Cards / Containers

- **Corner Style:** 8px radius (Rounded `md`).
- **Background:** Ivory on a Paper section; Pure-White on an Ivory section. Two-layer rule — never card-on-card-on-card.
- **Shadow Strategy:** None at rest. `state-hover` shadow on hover, with a 1px translateY.
- **Border:** 1px Rule (`#e6e3da`) at all states. The border is the load-bearing depth signal at rest.
- **Internal Padding:** 32px on standard cards; 24px on compact cards in dense grids; 40px on pricing tiers.
- **No nested cards.** Cards inside cards is always a layout mistake — restructure to siblings or use a divider.

### Inputs / Fields

- **Style:** 1px Rule border on a Pure-White background, 4px radius, 12×14 padding. Body type, charcoal text.
- **Placeholder:** Slate (`#7a7a7a`) — meets 4.5:1 on Pure-White.
- **Focus:** Border shifts to Healthcare Green; `state-focus` glow appears (Healthcare Green at 20% opacity, 3px ring); no inner shadow.
- **Error:** Border shifts to Signal Danger; an inline label below states the error in plain English, never just a red ring.
- **Forbidden:** Floating labels, inset shadows, gradient borders.

### Navigation

- **Top nav:** Fixed, transparent over the hero, then transitions to Ivory with a single `lift-nav` hairline once `scrollY > 50`. Backdrop-filter blur (12px) when sticky.
- **Logo lockup:** Sera mark + wordmark in Cormorant 800 — the wordmark is the only place Cormorant appears in a UI control rather than a content surface.
- **Nav links:** Instrument Sans 500 0.9rem, Graphite at rest, Charcoal on hover. Active state: Healthcare Green text with a 2px Healthcare Green underline 4px below the baseline.
- **CTA in nav:** Primary button shrunk to compact (10×20 padding). Always says "Book a Demo →" — never a generic verb.
- **Mobile:** A `<` 768px viewport collapses nav links into a hamburger that opens a full-width sheet (Pure-White, no overlay). Sheet items are 1rem 0.75rem padded, divided by Rule hairlines, ordered the same as desktop nav.

### Pricing Tier (signature component)

- **Default tier:** Ivory background, Rule border, 40px padding, Headline name in Cormorant, Metric price in Space Grotesk, Body description, Label-style "WHAT'S INCLUDED" eyebrow over a feature list.
- **Featured tier:** Charcoal background, Ivory text. The Headline tier name keeps Cormorant; the Metric price keeps Space Grotesk; the green accent appears only as a "RECOMMENDED" chip in Healthcare-Green-Tint sitting above the tier headline.
- **Feature list:** Each feature renders with a small Healthcare Green checkmark glyph (16px square) and Body-Small text. Excluded features render with a Slate em-dash glyph and Slate text — the absence is information.
- **CTA:** Each tier ends with a Primary button (or Ghost on the featured tier) labeled with what happens — "Start a 14-Day Trial" or "Talk to Sales," never "Choose Plan."

### Testimonial / Case-study card (signature component)

- **Layout:** Two columns on desktop — left holds a Metric block (`metric-block` token: $42K / 87% / 3.2 sec), right holds the quote and signature. Stacks on mobile, metric first.
- **Quote:** Body type, charcoal, no quotation marks. Bracketed by a single 32px Rule line above and below.
- **Signature:** "— Dr. Sarah Chen, Owner / Summit Physical Therapy / Boulder, CO" rendered in Body-Small, with the owner's name underlined in Terracotta. This is the single place Terracotta appears in the design system.
- **Forbidden:** Headshot photography, decorative quotation marks, star ratings, "verified by" badges.

### Section Eyebrow

A small but signature pattern: every major section opens with an ALL-CAPS Space Grotesk Label in Healthcare Green ("HOW IT WORKS," "RESULTS," "PRICING") sitting 16px above the Cormorant headline. The eyebrow is the only place green text appears at small size.

## 6. Do's and Don'ts

### Do:
- **Do** keep Healthcare Green to ≤10% of any rendered viewport. Use it on the brand mark, eyebrows, hover-state of primary buttons, success chips, positive metrics, and the focus ring. Nowhere else.
- **Do** tint every neutral toward warm gray — Ivory `#fbf9f3` over Pure-White, Charcoal `#1a1a1a` over `#000`. Pure values are forbidden.
- **Do** lead every section with an outcome ("Stop losing revenue to missed calls"), not a feature ("AI-powered call coverage").
- **Do** ship one, and only one, primary CTA per surface. Secondary actions defer to outline buttons or text links.
- **Do** use Space Grotesk for every numeric value — dollar amounts, percentages, phone numbers, time. Numbers are a category distinct from prose.
- **Do** lock body paragraph measures at 65–75ch.
- **Do** show specificity for trust — "Recovered $42K at Summit PT in Boulder, CO" — not "Trusted by 150+ clinics."
- **Do** design for the operator scrolling at 7am between patients on a phone. Mobile-first.

### Don't:
- **Don't** use bento-box grids, identical icon-heading-paragraph card stacks, or "minimal SaaS card" layouts. The category template is the AI-slop tell.
- **Don't** use purple-to-pink gradients, glowing orbs, neon-on-black, "powered by GPT" badges, or any visual that reads as 2024-AI-tool.
- **Don't** use generic clinic stock photos — smiling receptionists in scrubs, doctors fist-bumping, blurred-corridor backgrounds. Use real product UI, real call transcripts, real metric blocks.
- **Don't** use the category-reflex healthcare palette: teal-on-white, medical aqua, clinical cyan, mint green. Sera's green is darker, warmer, and rarer.
- **Don't** use vague CTAs — "Learn More," "Click Here," "Get Started," "Sign Up," "Choose Plan." Every CTA names what happens.
- **Don't** present hidden or "Contact Sales" pricing. Practice owners read that as expensive, opaque, slow.
- **Don't** use feature-language verbs — "supports," "enables," "leverages," "powers." Replace with outcomes — "answers," "books," "recovers," "covers."
- **Don't** use em dashes (—) in body copy. Use commas, colons, semicolons, periods, or parentheses. The Terracotta-underlined Em dash in testimonial signatures is the exception.
- **Don't** use side-stripe borders (`border-left: 4px solid …`) as accents on cards or callouts. Use full borders, background tints, or eyebrow labels instead.
- **Don't** use gradient text (`background-clip: text`). Single solid color, weight or size for emphasis.
- **Don't** use glassmorphism (`backdrop-filter: blur` decoratively). The sticky nav blur is the one allowed instance.
- **Don't** use modals as a first thought. Inline disclosure, expand-in-place, and progressive sections come before any dialog.
- **Don't** add shadows at rest. If a surface "needs a shadow to look right," its border or tint is wrong.
- **Don't** animate layout properties or use bounce/elastic easing. Ease-out-quart only; transforms and opacity only.
- **Don't** ship a flat scale of equal-weight type. The hierarchy depends on ≥1.4 ratio between Display, Headline, and Title.
