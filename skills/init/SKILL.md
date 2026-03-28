---
name: init
description: Initialize a new funnel project — set up project config with WordPress, n8n, payment, tracking, and design preferences
user_invocable: true
---

# Initialize Funnel Project

Set up the configuration for a new landing page / funnel project. This creates a `funnel-config.json` file that all other skills use.

## Instructions

1. Read the template at `funnel-builder-plugin/templates/funnel-config.example.json`
2. Ask the user the following questions (skip any they've already answered):

### Required
- **Project name** — client or project name
- **Language** — he (Hebrew), en (English), etc.
- **Direction** — rtl or ltr
- **WordPress URL** — the site where pages will be deployed
- **WordPress username** — for REST API deployment
- **Design theme** — dark or light
- **Primary color** — hex code for brand color
- **Funnel type** — free-trial, payment, or lead-only

### Optional (can be filled later)
- WordPress app password
- n8n URL and webhook path
- Payment provider details (SUMIT, Stripe, etc.)
- Tracking IDs (Facebook Pixel, TikTok, GTM, GA4)
- Turnstile keys
- Font preferences (heading + body)
- Trial duration and charge amount

3. Create `funnel-config.json` in the project root with the answers
4. Summarize what's configured and what still needs to be filled in

## Notes
- App passwords and API keys should be added by the user directly, not generated
- The config file is gitignored by default (contains secrets) — remind the user
- If user doesn't know their ACSS tokens, suggest they check WordPress → Automatic.css dashboard
