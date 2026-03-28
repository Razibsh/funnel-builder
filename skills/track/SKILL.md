---
name: track
description: Set up conversion tracking — Facebook Pixel, TikTok Pixel, Google Analytics, GTM, and consent mode
user_invocable: true
---

# Set Up Conversion Tracking

Configure pixel tracking, GTM, and consent mode for the funnel.

## Instructions

1. Read `funnel-config.json` for tracking IDs
2. Read `references/tracking-setup.md` for patterns
3. Determine what needs to be set up based on which IDs are provided
4. Generate the tracking code

## Setup Checklist

### GTM Container
- [ ] Create/configure GTM container
- [ ] Add GA4 Config tag
- [ ] Add Facebook Pixel Base tag
- [ ] Add TikTok Pixel Base tag (standalone if GTM template broken)
- [ ] Add Consent Mode handler (Complianz or similar)
- [ ] Add GTM snippet to WordPress (head + noscript)

### Frontend Events (in form JS)
- [ ] `InitiateCheckout` — when payment step shown
- [ ] `SubmitForm` — when form submitted
- [ ] `CompleteRegistration` — on successful webhook response
- [ ] Custom events as needed

### Server-Side Events (in n8n)
- [ ] `Purchase` via Facebook Conversions API — on successful charge
- [ ] Hash all PII with SHA256 before sending

### Consent Mode
- [ ] Default all consent types to `denied`
- [ ] Read consent cookies on page load
- [ ] Listen for consent grant/revoke events
- [ ] Map cookie categories to GTM consent types

## Output

Generate:
1. GTM tag configurations (description, not JSON export)
2. Frontend JS snippet for form events (to embed in Etch section script)
3. n8n Code node for server-side events (if applicable)
4. WordPress code snippet for GTM loading

## Notes

- Always check `typeof fbq === 'function'` before calling Facebook Pixel
- Always check `typeof ttq !== 'undefined'` before calling TikTok Pixel
- Fire conversion events AFTER confirmed success, not on form submit
- TikTok GTM template may be broken — have standalone snippet as fallback
