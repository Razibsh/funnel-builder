---
name: design
description: Create visual mockups using Stitch (Google) — design system setup, screen generation, and design-to-Etch conversion
user_invocable: true
---

# Design Landing Page

Create visual mockups using Stitch MCP before building in Etch.

## Instructions

1. Read `funnel-config.json` for design preferences (theme, colors, fonts, radius)
2. Read copy from `copy/{page-name}.md` if available
3. Create or select a Stitch design system matching the project config
4. Generate screens for each page section
5. Iterate with user feedback
6. When approved, hand off to `/funnel-builder:build` for Etch JSON generation

## Stitch MCP Tools

### Create Design System
Use `mcp__stitch__create_design_system` with project config:

```
colorMode: DARK or LIGHT (from config.design.theme)
headlineFont: Map config font to Stitch enum (e.g., Rubik → RUBIK)
bodyFont: Map config font to Stitch enum
customColor: config.design.primaryColor
roundness: Map config radius (5px → ROUND_FOUR, 8px → ROUND_EIGHT, 12px → ROUND_TWELVE)
```

### Generate Screens
Use `mcp__stitch__generate_screen_from_text` with detailed descriptions of each section.

### Generate Variants
Use `mcp__stitch__generate_variants` to create alternate designs for A/B testing.

## Design-to-Etch Mapping

| Stitch Element | Etch Equivalent |
|---------------|-----------------|
| Container | `section` with `data-etch-element="section"` |
| Card | `div` with card class + attached CSS |
| Button | `a` or `button` with button class |
| Input | `input` with field class |
| Image | `figure` > `img` pattern |
| Grid | `div` with CSS grid in attached style |
| Text | `etch/text` block inside `h1-h6` or `p` |

## Font Mapping (Stitch ↔ ACSS)

| Font Name | Stitch Enum | ACSS Variable |
|-----------|------------|---------------|
| Rubik | `RUBIK` | `var(--heading-font-family)` |
| Inter | `INTER` | `var(--body-font-family)` |
| Montserrat | `MONTSERRAT` | — |
| DM Sans | `DM_SANS` | — |
| Space Grotesk | `SPACE_GROTESK` | — |

## Notes

- Stitch generates Material Design-influenced layouts — Etch output should adapt these to match ACSS token system
- Stitch doesn't know about RTL — remind the user to flip layouts mentally for Hebrew
- Design is a starting point — the actual Etch JSON may differ in implementation details
- Save Stitch project ID in `funnel-config.json` for future reference
