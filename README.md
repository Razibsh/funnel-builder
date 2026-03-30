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

You need [Claude Code](https://claude.ai/code) (Anthropic's CLI tool) to use this plugin. If you don't have it yet, install it first.

### Step 1: Register the plugin source

Open Claude Code in your terminal and run:

```
/plugin marketplace add Razibsh/funnel-builder
```

This tells Claude Code where to find the plugin. You only need to do this once — it stays registered across all your projects.

### Step 2: Install the plugin

```
/plugin install funnel-builder@Razibsh
```

You'll see a screen asking where to install it. Choose one of:

- **User scope** — makes the plugin available in every project you open with Claude Code. Best for most people.
- **Project scope** — makes the plugin available for everyone who works on a specific project/repo. Good for teams.
- **Local scope** — makes the plugin available only for you, only in the current folder. Good for testing or keeping things separate.

That's it! The plugin is now installed.

### Step 3: Verify it works

Open any folder in Claude Code (or stay where you are) and type:

```
/funnel-builder:init
```

If the plugin loaded correctly, it will start the project setup wizard. If you see "unknown command", double-check that the install completed and you're in the right scope.

## Quick Start

Once installed, you have 7 slash commands — run them in order to build a complete funnel:

```
/funnel-builder:init          # Set up your project config
/funnel-builder:brief         # Define what you're building
/funnel-builder:copy          # Generate the landing page copy
/funnel-builder:design        # Create visual mockups
/funnel-builder:build         # Generate Etch page builder JSON
/funnel-builder:integrate     # Wire up n8n + payments
/funnel-builder:track         # Add conversion tracking
```

You can also run them individually if you only need one step (e.g., just tracking setup for an existing page).

## Uninstall

To remove the plugin:

```
/plugin uninstall funnel-builder@Razibsh
```

To also remove the marketplace registration:

```
/plugin marketplace remove Razibsh
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
