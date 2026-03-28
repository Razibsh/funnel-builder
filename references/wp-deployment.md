# WordPress Deployment Reference

## Deploying Etch Pages

### Method 1: Copy/Paste (recommended for new pages)
1. Generate Etch JSON
2. Open page in Etch editor
3. Cmd+V to paste the JSON
4. Save

### Method 2: REST API (for automation)

```bash
curl -X POST "https://your-site.com/wp-json/wp/v2/pages/{PAGE_ID}" \
  -u "username:app-password" \
  -H "Content-Type: application/json" \
  -d '{"content": "<!-- wp:etch/element -->...<!-- /wp:etch/element -->"}'
```

For FunnelKit landing pages, use `wffn_landing` post type:
```
POST /wp-json/wp/v2/wffn_landing/{ID}
```

### App Passwords
Generate in WordPress Admin → Users → Profile → App Passwords. Format: `xxxx xxxx xxxx xxxx xxxx xxxx` (spaces included).

## FunnelKit Caching Bug (Critical)

FunnelKit caches page content by slug/URL. Editing content via REST API or editor may NOT update what visitors see.

### Workaround
1. Edit content on a regular WordPress page first
2. Clone it into the funnel as a new landing page
3. **Change the slug** to force FunnelKit to serve fresh content

## WordPress Caching Layers

| Layer | How to Clear |
|-------|-------------|
| Redis Object Cache | `wp_cache_flush()` or Redis CLI |
| Etch compiled CSS | Re-paste section (but see style persistence bug) |
| Browser cache | `?nocache=timestamp` parameter |
| CDN/Cloudflare | Purge cache in Cloudflare dashboard |

Always test with `?nocache=timestamp` after deployment.

## Hebrew Text in REST API

Hebrew characters can get mangled in REST API deployment. Use HTML entities:

```
א = &#x05D0;   ב = &#x05D1;   ג = &#x05D2;   ד = &#x05D3;
ה = &#x05D4;   ו = &#x05D5;   ז = &#x05D6;   ח = &#x05D7;
ט = &#x05D8;   י = &#x05D9;   כ = &#x05DB;   ל = &#x05DC;
מ = &#x05DE;   נ = &#x05E0;   ס = &#x05E1;   ע = &#x05E2;
פ = &#x05E4;   צ = &#x05E6;   ק = &#x05E7;   ר = &#x05E8;
ש = &#x05E9;   ת = &#x05EA;
```

Better approach: base64-encode JS (which Etch does naturally) and keep Hebrew in `etch/text` content blocks (which handle UTF-8 fine).

## Code Snippets Plugin

Use WP Code Snippets for site-wide scripts (GTM, pixels). Key fields:
- **Placement:** `head-content` for tracking scripts
- **Include `<script>` tags** — the plugin inserts raw content
- **Priority:** Lower number = loads first

## Security Ninja

IP blocking after failed logins. If locked out:
1. Access via different IP or VPN
2. Unblock in Security Ninja settings
3. Or whitelist your IP in advance

## Automatic.css on FunnelKit Pages

ACSS may not load on FunnelKit checkout pages by default. Use a code snippet to exclude ACSS from FunnelKit checkout while keeping it on landing pages.

FunnelKit pages use `wffn_landing` post type. WordPress Customizer CSS does NOT load on them.
