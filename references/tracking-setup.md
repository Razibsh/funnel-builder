# Tracking & Conversion Setup

## GTM + Consent Mode v2

### Architecture
All tracking goes through Google Tag Manager (GTM). Individual pixel snippets are deactivated — GTM manages everything.

### Standard GTM Tags
1. **GA4 Config** — trigger: Initialization - All Pages
2. **Facebook Pixel Base** — trigger: All Pages
3. **TikTok Pixel Base** — trigger: All Pages (Note: GTM TikTok template may be broken — use standalone snippet as fallback)
4. **Microsoft Clarity** — trigger: All Pages
5. **Consent Mode Handler** — trigger: Consent Initialization - All Pages

### Consent Mode Setup (Complianz)
Default ALL consent types to `denied`. Read Complianz cookies to grant:

| Complianz Cookie | GTM Consent Type |
|-----------------|-----------------|
| `cmplz_marketing` | `ad_storage`, `ad_user_data`, `ad_personalization` |
| `cmplz_statistics` | `analytics_storage` |
| `cmplz_preferences` | `personalization_storage` |
| `cmplz_functional` | `functionality_storage` |

`security_storage` always granted.

Listen for `cmplz_fire_categories` (accept) and `cmplz_revoke` (revoke) events.

### Loading GTM
Two snippets needed:
1. Head script (WP Code Snippet, high priority): loads `gtm.js`
2. Noscript tag (after `<body>`): fallback for no-JS

## Funnel Events

### Lead Capture Funnel

| Step | Event | Where |
|------|-------|-------|
| Page load | `PageView` | Auto (GTM) |
| Form step 1 complete | `SubmitForm` (TikTok) | Frontend JS |
| Card details shown | `InitiateCheckout` (FB + TikTok) | Frontend JS |
| Card tokenized | `AddPaymentInfo` (TikTok) | Frontend JS |
| Webhook success | `CompleteRegistration` (FB standard) | Frontend JS |
| Webhook success | `Subscribe` (TikTok) | Frontend JS |
| 30-day charge | `Purchase` (FB CAPI) | Server-side (n8n) |

### Payment Funnel

| Step | Event | Where |
|------|-------|-------|
| Page load | `PageView` | Auto |
| Form submit | `Lead` (FB) | Frontend JS |
| Payment success | `Purchase` (FB) | Frontend JS |
| Payment success | `CompletePayment` (TikTok) | Frontend JS |

## Facebook Pixel

### Frontend Events
```javascript
if (typeof fbq === 'function') {
  fbq('track', 'CompleteRegistration');
  // Or custom event:
  fbq('trackCustom', 'SubscribedFreeTrial', {
    currency: 'ILS',
    value: 0
  });
}
```

### Advanced Matching
Send hashed user data for better attribution:
```javascript
fbq('init', 'PIXEL_ID', {
  em: userEmail,
  fn: firstName,
  ln: lastName,
  ph: phone
});
```

### Server-Side (Conversions API)
For Purchase events after billing, use Facebook Conversions API from n8n. Hash all PII with SHA256 before sending.

**Important:** n8n task runner blocks `require('crypto')` and `crypto.subtle`. Use a pure JS SHA256 implementation.

## TikTok Pixel

```javascript
if (typeof ttq !== 'undefined') {
  ttq.track('SubmitForm');
  ttq.track('InitiateCheckout', {
    content_type: 'product',
    currency: 'ILS',
    value: 0
  });
}
```

**Note:** TikTok's GTM template may not work. If so, use a standalone `<script>` snippet loaded via WP Code Snippets (with `<script>` tags, head-content placement).

## Best Practices

1. Fire conversion events AFTER confirmed success (webhook response), not on form submit
2. Use custom events for intermediate steps, standard events for final conversions
3. Always check `typeof fbq === 'function'` / `typeof ttq !== 'undefined'` before calling
4. Send UTM parameters from frontend to n8n for attribution tracking
5. Test with Facebook Pixel Helper and TikTok Pixel Helper browser extensions
