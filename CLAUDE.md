# Bare & Bloom — Shopify Theme (Dawn Fork)
> Read this file at the start of every session. It contains the full brand brief, technical decisions, progress tracker, and design references.

---

## Project Overview
**Store:** vayn-series.myshopify.com  
**Theme:** Custom fork of Shopify Dawn  
**Stack:** Liquid, CSS (no preprocessors), vanilla JS, Shopify CLI  
**Goal:** Build a premium maternity and nursing wear storefront that feels like HATCH Collection meets Toteme — editorial, warm, and confident. No page builders. Everything in native Dawn code.

---

## Brand Identity

### Colors
```css
--bb-cream:      #FAF6F1   /* page background */
--bb-blush:      #F2E4DC   /* soft sections, cards */
--bb-petal:      #EDD5CA   /* announcement bar text, accents */
--bb-rose:       #D9A89A   /* mid-tone accent */
--bb-terracotta: #B5685A   /* primary brand color, CTAs, hover states */
--bb-sage:       #8FA88A   /* trust icons, nature accents */
--bb-warm-grey:  #9B8F8A   /* muted text, nav links default state */
--bb-charcoal:   #2E2724   /* headings, body text, footer background */
```

### Typography
- **Headlines:** Cormorant Garamond — serif, elegant, italic used for brand moments
- **Body / UI / Buttons:** Jost — clean sans-serif, lightweight (300), tracked uppercase for CTAs
- **Google Fonts URL:**
```
https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;1,300;1,400;1,500&family=Jost:wght@300;400;500&display=swap
```

### Brand Feel
Premium, warm, minimal. Generous whitespace. Editorial photography. The customer is an expectant or nursing mother who wants to feel beautiful and supported. Every design decision should feel considered and refined — never generic or clinical.

### Reference Brands
HATCH Collection, Seraphine, Toteme, Reformation

---

## Design Reference Files
These HTML files live in `_design-refs/` inside this repo. Claude Code can read them directly for exact CSS values, layouts, and design intent.

| File | What it shows |
|---|---|
| `_design-refs/bare-bloom-store.html` | Full homepage desktop design |
| `_design-refs/bare-bloom-product-page.html` | Full product page desktop design |
| `_design-refs/bare-bloom-mobile.html` | Mobile layouts for homepage + product page |
| `_design-refs/bare-bloom-brand-kit.html` | Color palette, typography, and component reference |

**When building or restyling any section, read the relevant design ref file first.**

---

## CSS Architecture

### Variable naming convention
All Bare & Bloom custom properties are prefixed `--bb-` to avoid collisions with Dawn's own variables.

### Specificity strategy
We never use `!important`. Instead we beat Dawn's specificity cleanly:
- Dawn component CSS: typically `(0,1,0)` or `(0,2,0)`
- Our overrides in `base.css`: use `(0,2,0)` to `(0,4,0)` selectors
- Dynamic Liquid values go in `{%- style -%}` blocks scoped to `#section-{{ section.id }}`
- Static CSS goes in `<style>` blocks or `base.css`

### Where our code lives
All brand styles are appended to `assets/base.css` in clearly labelled blocks. Do not create separate CSS files unless building a standalone new component.

---

## What Has Been Built

### ✅ Task 1 — Brand Foundations
**Files:** `assets/base.css`, `layout/theme.liquid`
- Cormorant Garamond + Jost loaded via Google Fonts (with preconnect)
- All `--bb-*` color tokens and font variables in `:root`
- Dawn's own font variables overridden to point to Jost

### ✅ Task 2 — Global Typography
**Files:** `assets/base.css`
- Cormorant Garamond on h1–h6, `.h0`–`.hxxl` with editorial weights, tracking, line-heights
- Jost on body, `.rte`, buttons (uppercase tracked), nav items
- Font smoothing for crisp retina rendering

### ✅ Task 3 — Header
**Files:** `assets/base.css`, `sections/header.liquid`
- Cream background (`--bb-cream`) with backdrop blur
- Terracotta nav hover, charcoal default
- Always-sticky with subtle scroll shadow
- Charcoal icons, terracotta cart badge
- Cream mobile drawer
- Schema defaults: sticky always, separator off, 24px padding top/bottom

### ✅ Task 4 — Hero Section (custom)
**Files:** `sections/bb-hero.liquid` (new file, 460 lines)
- 50/50 split desktop, stacked mobile (image top at 50vw height)
- Staggered fade-up animation with expo-out easing (`cubic-bezier(0.16, 1, 0.3, 1)`)
- Animation order: label (0.08s) → heading (0.22s) → subtext (0.38s) → CTAs (0.52s) → image (0.05s fade only)
- `prefers-reduced-motion` safe
- Full schema: image picker, overlay opacity (0–30%, default 8%), min-height, focal point, both CTA buttons
- Added to `templates/index.json` as first section

### ✅ Task 5 — Featured Collection + Product Cards
**Files:** `assets/base.css`
- Centered Cormorant Garamond section heading
- Cream card backgrounds, borders and shadows zeroed
- Terracotta price color
- Terracotta quick-add button
- Smooth 650ms image zoom on hover, lift shadow

### ✅ Task 6 — Footer
**Files:** `sections/footer.liquid`, `assets/base.css`
- Charcoal background using CSS var cascade (no `!important`)
- Tagline: "Designed for the journey of motherhood" in Cormorant Garamond italic, centered
- Thin terracotta divider (55% opacity) separating tagline from columns
- Column headings: Cormorant Garamond, uppercase, tracked, cream
- Links: Jost 300, cream 72%, terracotta on hover
- Newsletter input: translucent cream on charcoal, terracotta focus ring
- Bottom bar: copyright left (Jost 300, cream 50%), payment icons right (60% opacity, lifts to 85% on hover)

### ✅ Task 7 — Product Page
**Files:** `assets/base.css`, `sections/main-product.liquid`
- 55/45 split: images left, info panel right
- Sticky info panel (`top: 8rem` to clear sticky header)
- Image borders/shadows zeroed via CSS vars
- Main image: `scale(1.04)` zoom on hover
- Selected thumbnail: 2px solid terracotta border
- Product title: Cormorant Garamond, `clamp(3.2rem, 3.2vw, 4.8rem)`, charcoal
- Price: Jost 1.8rem, terracotta; compare-at struck through, muted
- Hairline charcoal divider below price
- Variant pills: full border-radius, terracotta fill on `:checked`
- Add to cart: full width, terracotta fill, cream Jost uppercase, inverts on hover
- Wishlist text link: muted charcoal, terracotta hover
- Description: Jost 1.45rem, line-height 1.75, charcoal 68%
- Accordions: thin rgba hairline dividers, Jost 500 uppercase headings

---

## What Comes Next (in order)

### 🔲 Task 8 — Trust Signals Snippet
**Target files:** `snippets/bb-trust-signals.liquid` (new), `sections/main-product.liquid`
- 3 icons in a row: "Free Shipping over R800" / "Easy Returns" / "Ethically Made"
- Inline SVG icons in `--bb-sage`
- Jost small, charcoal text
- Flex row, centered, evenly spaced
- Cream background, subtle terracotta top border hairline
- Inserted below add-to-cart button in `main-product.liquid`

### 🔲 Task 9 — Collection Page
**Target files:** `sections/main-collection-product-grid.liquid`, `assets/base.css`
- Clean grid: 3 columns desktop, 2 columns tablet, 1 column mobile
- Filter/sort bar: minimal, Jost, terracotta active state
- Collection hero banner: blush background, Cormorant Garamond heading, centered
- Pagination or infinite scroll styling
- Breadcrumb: Jost small, warm-grey

### 🔲 Task 10 — Mobile Refinements
**Reference:** `_design-refs/bare-bloom-mobile.html`
- Hero: image fills top 50vw, text stacks below with tighter padding
- Header: hamburger left, logo centered, cart right
- Product page: single column, image gallery swipeable
- Cards: 2-column grid, tighter spacing
- Footer: single column stack
- All touch targets minimum 44px

### 🔲 Task 11 — Announcement Bar
- Charcoal background, petal (`#EDD5CA`) text
- Jost, 0.25em tracking, uppercase, small (0.7rem)
- Rotating messages: "Free shipping over R800" / "New arrivals weekly" / "Designed for every stage"

### 🔲 Task 12 — Push to Shopify Store
```bash
shopify theme push --store vayn-series.myshopify.com --unpublished
```
- Push as unpublished theme named "Bare & Bloom v1"
- Review in Shopify admin before publishing
- Upload hero image and product images via Theme Editor

---

## Daily Workflow

### Starting a session
```bash
# Terminal 1 — dev server
cd Desktop\bare-bloom
shopify theme dev --store vayn-series.myshopify.com

# Terminal 2 — Claude Code
cd Desktop\bare-bloom
claude
```
Preview URL: http://127.0.0.1:9292

### Ending a session
```bash
# In Terminal 2
/exit

# Save to GitHub
git add .
git commit -m "Description of what was built"
git push

# In Terminal 1
Ctrl+C
```

---

## Key Decisions & Constraints
- **No `!important`** — beat specificity cleanly with longer selectors
- **No page builders** — everything in native Liquid/CSS/JS
- **Schema settings on everything** — all sections must be editable in the Shopify theme editor
- **Dawn conventions** — follow Dawn patterns: `{%- style -%}` for Liquid-computed values, `section.index == 1` for LCP, `placeholder_svg_tag` for blank images
- **Mobile first** — all new CSS written mobile-first with min-width breakpoints matching Dawn's: 750px (tablet), 990px (desktop), 1200px (wide)
- **Accessibility** — `prefers-reduced-motion` on all animations, `aria-hidden` on decorative elements
