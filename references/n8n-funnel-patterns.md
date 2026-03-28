# n8n Funnel Integration Patterns

Proven webhook → CRM → billing patterns for landing page funnels.

## Core Architecture

```
Frontend Form
  → POST to n8n Webhook
    → Validate (secret + Turnstile + honeypot)
    → Mode Switch (payment vs free-trial)
    → Process (create customer, save token, etc.)
    → Respond to frontend
    → Post-response (CRM, membership, WhatsApp, tracking)
```

## Security Layer (always include)

### 1. Webhook Secret
Frontend sends `X-Webhook-Secret` header. n8n validates it matches.

```javascript
// Frontend
fetch(webhookUrl, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-Webhook-Secret': 'your-secret-here'
  },
  body: JSON.stringify(data)
});
```

### 2. Cloudflare Turnstile
Frontend renders Turnstile widget, sends token. n8n verifies with Cloudflare API.

```javascript
// n8n: Verify Turnstile (HTTP Request node)
// POST https://challenges.cloudflare.com/turnstile/v0/siteverify
// Body: { secret: "your-secret", response: turnstileToken }
```

### 3. Honeypot
Hidden field + timestamp. Bot detection in n8n:

```javascript
// Frontend: hidden field
<input type="text" name="website" style="position:absolute;left:-9999px">

// Also send: formLoadedAt: Date.now()

// n8n: Check if honeypot filled OR form submitted too fast (< 3 seconds)
```

## Mode Switch Pattern

Use a Switch node to route by `body.mode`:

| Mode | Route |
|------|-------|
| `free-trial` | Dedup → SUMIT create customer + save token → respond → CRM → membership |
| `payment` | SUMIT charge → respond → CRM |
| `lead-only` | Respond → CRM |

## Free Trial Flow

```
1. Dedup Check (search Airtable by phone)
2. Has Active Trial? → Yes: Respond "already registered"
3. No: SUMIT Create Customer + Save Token
4. Respond Trial Success (don't make user wait for CRM)
5. Prepare Trial Lead Data
6. Send to CRM Workflow (sub-workflow)
7. Post-CRM: Update contact + Close enquiry + Create membership
8. Send Welcome WhatsApp
```

### Key lesson: Use CRM output directly
The CRM sub-workflow returns `enquiryRecordId`. Use it directly to close the enquiry instead of searching Airtable — search has index lag and may fail.

```javascript
// GOOD — use the ID from CRM output
let enquiryId = crmResult.enquiryRecordId || null;

// FALLBACK — search only if CRM didn't return ID
if (!enquiryId) {
  // search Airtable for open enquiries...
}
```

## Payment Flow

```
1. SUMIT Charge (with SingleUseToken from frontend)
2. Charge OK? → No: Respond Failure
3. Yes: Prepare Lead Data
4. Respond Success
5. Send to CRM Workflow
```

## SUMIT API Calls

### Tokenize Card (frontend, no charge)
```
POST /creditguy/vault/tokenizesingleusejson/
Body: { Credentials: { CompanyID, APIPublicKey }, CardNumber, ExpirationMonth, ExpirationYear, CVV }
Returns: { Data: { SingleUseToken } }
```

### Create Customer
```
POST /accounting/customers/create/
Body: { Credentials: { CompanyID, APIKey }, Name, Email, Phone, SearchMode: 0 }
```

### Save Payment Method
```
POST /billing/paymentmethods/setforcustomer/
Body: { Credentials, Customer: { ID }, CreditCard: { SingleUseToken } }
```

### Charge
```
POST /billing/payments/charge/
Body: { Credentials, Customer: { ID }, Items: [...], SendDocumentByEmail: true }
```

### Set Up Recurring (for trial conversion)
```
POST /billing/recurring/charge/
Body: { Credentials, Customer: { ID }, Items: [...], OnlyDocument: true }
Returns: { Data: { RecurringCustomerItemIDs: [...] } }
```

**Note:** `RecurringCustomerItemIDs` is a plural ARRAY. Read `[0]`.

## Webhook Payload Structure

Standard payload from frontend to n8n:

```json
{
  "mode": "free-trial",
  "singleUseToken": "uuid-from-sumit",
  "name": "Full Name",
  "phone": "0541234567",
  "email": "user@example.com",
  "goal": "selected-goal",
  "amount": 0,
  "source": "WEBSITE::PAGE-NAME",
  "trialDays": 30,
  "productId": "airtable-product-record-id",
  "utmSource": "",
  "utmMedium": "",
  "utmCampaign": "",
  "timestamp": "2026-01-01T00:00:00.000Z",
  "turnstileToken": "cloudflare-token",
  "honeypotValue": "",
  "formLoadedAt": 1234567890
}
```

## Error Handling Pattern

```
Error Trigger → Parse Error Details → Telegram Alert → Wait (retry delay) → Check If Completed → Should Retry? → Recovery Action
```

Always send errors to Telegram/Slack for visibility.

## Phone Normalization (Israeli)

```javascript
// Starts with 0 → +972 + rest
// 9 digits → prepend +972
// Otherwise → prepend + if missing
const phone = raw.replace(/\D/g, '');
const clean = phone.startsWith('0')
  ? '+972' + phone.slice(1)
  : phone.startsWith('972')
    ? '+' + phone
    : phone;
```

## WhatsApp via Green API

Use the community node `@green-api/n8n-nodes-whatsapp-greenapi`. Send messages after the webhook response so the user doesn't wait.

## Facebook Conversions API (Server-Side)

For server-side events (like Purchase after billing), use a Code node with pure JS SHA256 (n8n task runner blocks `require('crypto')` and `crypto.subtle`).
