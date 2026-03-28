---
name: integrate
description: Set up n8n webhook handler, payment flow (SUMIT/Stripe), CRM pipeline, and WhatsApp notifications for the funnel
user_invocable: true
---

# Integrate Funnel Backend

Set up the n8n workflow that handles form submissions, payment processing, CRM updates, and notifications.

## Instructions

1. Read `funnel-config.json` for n8n URL, payment provider, webhook secret
2. Read `references/n8n-funnel-patterns.md` for proven patterns
3. Determine the funnel type from config: `free-trial`, `payment`, or `lead-only`
4. Build or update the n8n workflow

## Funnel Types

### Free Trial
```
Webhook → Validate → Dedup Check → SUMIT (create customer + save token)
  → Respond Success → CRM → Close Enquiry → Create Membership → WhatsApp
```

### Payment
```
Webhook → Validate → SUMIT Charge → Respond Success → CRM
```

### Lead Only
```
Webhook → Validate → Respond Success → CRM
```

## Security Layer (always include)

1. **Webhook Secret** — `X-Webhook-Secret` header validation
2. **Cloudflare Turnstile** — verify token with Cloudflare API
3. **Honeypot** — check hidden field value + form load timestamp

## Key Patterns to Follow

### Respond BEFORE post-processing
Don't make the user wait for CRM, WhatsApp, etc. Respond to the webhook immediately after the core action (tokenize/charge), then do everything else after.

### Use CRM output for enquiry closing
When the CRM sub-workflow returns, it includes `enquiryRecordId`. Use this directly to close the enquiry — don't search Airtable (search has index lag).

### Error handling
Always add: Error Trigger → Parse Details → Telegram/Slack Alert

### Phone normalization
Israeli: `0xx` → `+972xx`. Always normalize before CRM lookup.

## Frontend JavaScript Required

The form JS must:
1. Collect form data
2. Tokenize card via SUMIT API (if payment/trial)
3. Send webhook with standard payload structure
4. Handle response (show success/error)
5. Fire tracking events on success

See `references/n8n-funnel-patterns.md` for the standard webhook payload structure.

## Billing Scheduler (for free trials)

Separate workflow, runs daily:
1. Query Airtable for expired trials
2. Create SUMIT recurring charge
3. Mark as converted
4. Create paid membership
5. Send FB CAPI Purchase event
6. Telegram notification

## n8n MCP Tools

Use these tools to build/modify workflows:
- `n8n_create_workflow` — create new workflow
- `n8n_update_partial_workflow` — add/update nodes incrementally
- `n8n_get_workflow` — inspect existing workflows
- `n8n_test_workflow` — test with sample data
- `n8n_validate_workflow` — check for errors
