---
name: build
description: Generate Etch page builder JSON sections for landing pages — hero, features, forms, pricing, FAQ, testimonials, CTA
user_invocable: true
---

# Build Etch Page Sections

Generate production-ready Etch JSON that can be pasted directly into the Etch page builder.

## Instructions

1. Read `funnel-config.json` for project design settings (theme, colors, fonts, direction)
2. Read ALL reference docs before generating:
   - `references/etch-json-format.md` — block structure, innerHTML rules, style system
   - `references/etch-gotchas-and-best-practices.md` — CRITICAL, avoid known pitfalls
   - `references/acss-tokens.md` — use proper ACSS tokens with fallbacks
3. Ask the user what section to build (or use copy from `/funnel-builder:copy` output)
4. Generate the Etch JSON section

## Section Types

- **Hero** — headline, subtitle, CTA button, optional background image/video
- **Features** — grid of feature cards with icons
- **Stats Bar** — animated counters
- **Signup Form** — multi-step form (goals → contact info → payment)
- **Pricing** — price cards with comparison
- **Testimonials** — customer quotes with photos
- **FAQ** — accordion-style questions
- **About** — team/brand section
- **Final CTA** — bottom conversion section
- **Timeline** — step-by-step process

## Mandatory Rules

### JSON Structure
- Every section: `data-etch-element: "section"` + `"etch-section-style"` in styles
- Every block: `metadata.name` (descriptive, for editor tree)
- Random 7-char alphanumeric style IDs
- `innerContent` null count MUST match `innerBlocks` length
- Leaf nodes: `innerHTML: ""`, `innerContent: []`

### CSS
- ALWAYS use ACSS tokens with fallbacks: `var(--primary, #edad24)`
- Use `color-mix(in oklch, ...)` for transparency (NOT `--trans-` tokens)
- RTL: use `text-align: right` (NOT `text-align: end`)
- Mobile-safe: no `white-space: nowrap`, no unconstrained `inline-flex`
- Add `overflow: hidden` on sections
- For flex rows, don't use `data-etch-element="flex-div"` — use plain `div` with flexbox class

### JavaScript
- Wrap in IIFE with `'use strict'`
- Add builder detection: `if(window!==window.top){if(window.frameElement?.id==='etch-iframe')return;}`
- Base64-encode and put in `attrs.script.code`
- NO `&&` in inline JS (WordPress encodes it) — use `?.` instead
- For elements that should be hidden on load but editable in builder: remove `display: none` from Etch CSS, let JS hide them at runtime

### Text
- Use `<p>` and `<div>` for text containers (NOT `<span>` — invisible in Etch)
- Hebrew text works fine in `etch/text` content
- For REST API deployment, encode Hebrew as HTML entities in HTML blocks

### Images
- Use figure+img pattern (NOT bare img)
- Include `loading="lazy"` and descriptive `alt`

## Output Format

Output a complete JSON object ready to paste:

```json
{
  "type": "block",
  "gutenbergBlock": { ... },
  "styles": { ... },
  "components": {}
}
```

Save as `sections/{section-name}.json` in the project folder.
