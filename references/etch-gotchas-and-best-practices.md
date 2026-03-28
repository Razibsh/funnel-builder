# Etch Gotchas & Best Practices

Hard-won lessons from building production landing pages with Etch. Follow these to avoid hours of debugging.

## Critical: Style Persistence Bug

When you paste an Etch JSON section, delete it, and paste a new version:
- **Old styles from the first paste PERSIST** in the page's compiled CSS
- New styles may NOT override them (different random style IDs)
- HTML updates, JS updates, but CSS stays stale

### Workaround
1. Use Etch attached styles for editor appearance (so you see correct look while editing)
2. ALSO inject critical layout/visibility CSS via JavaScript as a fallback
3. For elements hidden by JS, remove `display: none` from Etch CSS and let JS handle visibility — this way elements remain visible and editable in the builder

```javascript
// Inject critical CSS via JS to survive style cache issues
var s = document.createElement('style');
s.textContent = ".my-section { background: var(--base, #110a00); }";
document.head.appendChild(s);
```

## Critical: CSS Tokenizer

Etch auto-transforms CSS values into ACSS tokens on import. It does NOT preserve raw values.

| You write | Etch transforms to |
|-----------|-------------------|
| `border-radius: 1.5rem` | `border-radius: var(--radius-xxl)` |
| `var(--radius, 1.5rem)` | `var(--radius-xxl)` |
| `var(--space-xl)` | `var(--space-l)` (may remap!) |

### How to prevent unwanted remapping
- Use ACSS tokens directly: `var(--radius-xl)` instead of raw values
- For custom values, use private custom properties: `--_my-radius: 1.5rem; border-radius: var(--_my-radius);`
- Etch doesn't recognize `--_` prefixed properties as ACSS tokens

## Critical: Compound Selectors Strip Classes

When a style has a compound selector (e.g., `.form__input[dir="ltr"]`), Etch can't extract the class name and renders the element WITHOUT any class attribute.

### Fix
All elements must include a style with a SIMPLE selector (e.g., `.form__input`) in their styles array. Add compound-selector styles as additional entries.

## Critical: Nested Hover Rules Get Stripped

```css
/* THIS GETS STRIPPED on import: */
& :hover .child-class { color: red; }

/* THIS IS PRESERVED: */
&:hover { color: red; }
```

Only self-targeting hover rules survive. For child hover effects, use separate styles or Etch interactions.

## Critical: flex-div Defaults to Column

`data-etch-element="flex-div"` forces `flex-direction: column` and `inline-size: 100%`. CSS overrides alone DON'T reliably change this.

### Fix
- After pasting JSON, click the "flex row" icon in Etch toolbar
- OR: don't use `data-etch-element="flex-div"` — use a plain `div` and set flexbox via your own class

## Critical: span Tags Don't Render Text

`<span>` tags don't render text content in Etch. Always use `<p>` or `<div>` for text containers. `<ul>`/`<li>` also don't work well — use `<div>` for list-like structures.

## Critical: WordPress && in Inline JS

WordPress encodes `&&` as `&#038;&#038;` in inline scripts, breaking JavaScript. Use optional chaining (`?.`) instead:

```javascript
// BAD — breaks on WordPress:
if (a && b) { ... }

// GOOD — works everywhere:
if (a?.valueOf() ? b : false) { ... }
```

For Etch section scripts, this doesn't apply since the JS is base64-encoded. But for HTML blocks or inline scripts, always avoid `&&`.

## Important: ACSS Fallback Pattern

ACSS tokens WITHOUT fallbacks fail on regular WordPress pages and some FunnelKit contexts. Always use fallbacks:

```css
/* BAD — may fail: */
background: var(--base);

/* GOOD — always works: */
background: var(--base, #110a00);
```

## Important: RTL text-align

`text-align: end` in RTL context = LEFT alignment. Always use `text-align: right` explicitly for Hebrew/Arabic content.

## Important: Caching

Always test with `?nocache=timestamp` after deploying. Caching layers:
1. Redis Object Cache (WordPress)
2. Etch compiled CSS (persists across re-pastes)
3. Browser cache
4. CDN/Cloudflare

## Important: FunnelKit Caching Bug

FunnelKit caches page content by slug/URL. Editing content via REST API or editor may NOT update what visitors see. Workaround: change the slug to force fresh content.

## Best Practices for JSON Generation

1. Always set `metadata.name` — makes blocks findable in the editor tree
2. Use `"etch-section-style"` in section-level styles array
3. Generate random 7-char alphanumeric style IDs
4. Match `innerContent` null count to `innerBlocks` length
5. Wrap JS in IIFE with builder detection
6. Base64-encode all section scripts
7. Use figure+img pattern for images (not bare img)
8. For elements that JS hides/shows, keep display visible in Etch CSS so they're editable in the builder, and let JS handle runtime visibility

## Mobile-Safe CSS Rules

- No `white-space: nowrap` (causes horizontal overflow)
- No unconstrained `inline-flex` (items won't wrap)
- Add `overflow: hidden` on sections
- Use `max-width: 100%` on fixed-width elements
- Test on 375px viewport minimum
