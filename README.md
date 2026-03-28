# Funnel Builder Plugin for Claude Code

End-to-end landing page and funnel builder. Goes from business brief to live page with payment processing and conversion tracking.

## What it does

| Phase | Skill | Description |
|-------|-------|-------------|
| 1 | `/funnel-builder:init` | Set up project config (WordPress, n8n, payments, tracking) |
| 2 | `/funnel-builder:brief` | Gather business info and create project brief |
| 3 | `/funnel-builder:copy` | Generate landing page copy (headlines, CTAs, forms, FAQ) |
| 4 | `/funnel-builder:design` | Create visual mockups with Stitch |
| 5 | `/funnel-builder:build` | Generate Etch page builder JSON sections |
| 6 | `/funnel-builder:integrate` | Set up n8n webhooks, payments, CRM |
| 7 | `/funnel-builder:track` | Configure pixels, GTM, consent mode |

Or use the orchestrator agent to be guided through all phases.

## Tech Stack

- **Page Builder:** [Etch](https://etchwp.com) + [Automatic.css](https://automaticcss.com)
- **CMS:** WordPress (with FunnelKit for funnels)
- **Backend:** [n8n](https://n8n.io) (workflow automation)
- **Payments:** SUMIT, Stripe (extensible)
- **Tracking:** Facebook Pixel, TikTok Pixel, GA4, GTM
- **Design:** Stitch (Google) via MCP

## Install

```
/plugin install your-username/funnel-builder
```

## Quick Start

```
/funnel-builder:init          # Set up your project
/funnel-builder:brief         # Define what you're building
/funnel-builder:copy          # Generate the copy
/funnel-builder:design        # Create mockups
/funnel-builder:build         # Generate Etch JSON
/funnel-builder:integrate     # Wire up n8n + payments
/funnel-builder:track         # Add conversion tracking
```

## What's inside

### Skills (7)
Custom slash commands for each phase of the funnel build process.

### References (5)
Battle-tested documentation covering every gotcha, best practice, and pattern discovered while building production funnels. Includes:
- Etch JSON format and block structure
- 15+ critical gotchas (style persistence, CSS tokenizer, flex-div defaults, etc.)
- ACSS token system with dark/light theme patterns
- n8n webhook security and payment flow patterns
- GTM consent mode and pixel tracking setup
- WordPress deployment and caching strategies

### Templates
- Project config template
- (Coming soon) n8n workflow templates, GTM container export

## License

MIT
