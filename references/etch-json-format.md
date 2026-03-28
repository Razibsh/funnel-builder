# Etch JSON Format Reference

## Block Types (11 total)

| Block Name | Purpose | Has Children? |
|------------|---------|---------------|
| `etch/element` | Any HTML element (div, section, h1, a, img, etc.) | Yes |
| `etch/text` | Plain text content (no HTML) | No (leaf) |
| `etch/raw-html` | Rich text with HTML (strong, a, etc.) | No (leaf) |
| `etch/component` | Instance of a reusable component | Varies |
| `etch/condition` | Conditional rendering wrapper | Yes |
| `etch/dynamic-element` | Element with dynamic tag | Yes |
| `etch/dynamic-image` | WordPress media library image | No (leaf) |
| `etch/svg` | SVG icon from URL or data URI | No (leaf) |
| `etch/loop` | Iterate over data source | Yes |
| `etch/slot-placeholder` | Named insertion point inside components | No |
| `etch/slot-content` | Content provider for named slots | Yes |

## etch/element (primary block)

```json
{
  "blockName": "etch/element",
  "attrs": {
    "metadata": { "name": "Descriptive Name" },
    "tag": "section",
    "attributes": {
      "class": "my-section",
      "data-etch-element": "section",
      "dir": "rtl"
    },
    "styles": ["etch-section-style", "a1b2c3d"]
  },
  "innerBlocks": [ ... ],
  "innerHTML": "\n\n",
  "innerContent": ["\n", null, "\n"]
}
```

**Valid tags:** section, div, h1-h6, p, a, button, span, ul, ol, li, img, figure, figcaption, nav, header, footer, article, form, input, label, pre, br

**Required attrs:** `metadata.name`, `tag`, `attributes` (can be `{}`), `styles` (can be `[]`)

## etch/text (leaf)

```json
{
  "blockName": "etch/text",
  "attrs": {
    "metadata": { "name": "Text" },
    "content": "Your text here"
  },
  "innerBlocks": [],
  "innerHTML": "",
  "innerContent": []
}
```

## etch/raw-html (rich text leaf)

```json
{
  "blockName": "etch/raw-html",
  "attrs": {
    "metadata": { "name": "Rich Text" },
    "content": "<strong>Bold</strong> and <a href=\"#\">linked</a>"
  },
  "innerBlocks": [],
  "innerHTML": "",
  "innerContent": []
}
```

## innerHTML / innerContent Rules

These tell Etch WHERE child blocks sit inside the parent:

- **`innerHTML`**: Whitespace string with `\n` between children
- **`innerContent`**: Array alternating between whitespace strings and `null` (where each `null` = one child block)

### Pattern for N children:

```
innerHTML = "\n" + "\n\n".repeat(N-1) + "\n"
innerContent = ["\n", null, "\n\n", null, "\n\n", null, "\n"]
```

Example — 3 children:
```json
"innerHTML": "\n\n\n\n\n\n",
"innerContent": ["\n", null, "\n\n", null, "\n\n", null, "\n"]
```

**Leaf nodes** (etch/text, etch/raw-html): always `innerHTML: ""` and `innerContent: []`

## Style System

Styles live WITH the element (attached CSS model). When you copy/paste, styles travel with it.

```json
"styles": {
  "a1b2c3d": {
    "type": "class",
    "selector": ".my-section__heading",
    "collection": "default",
    "css": "font-size: var(--h1, 3rem);\n  font-weight: 900;",
    "readonly": false
  }
}
```

### Style types

| type | Purpose | Example selector |
|------|---------|------------------|
| `"element"` | System/readonly styles | `:where([data-etch-element="section"])` |
| `"class"` | Custom class styles | `.hero__heading` |
| `"id"` | ID-based styles | `#discovery` |
| `"custom"` | Arbitrary selectors | `body figure` |

### Style IDs

Generate random 7-character alphanumeric IDs (e.g., `"a1b2c3d"`). Each element references style IDs in its `styles` array.

## Section Structure (top-level)

Every section MUST have `data-etch-element: "section"` and include `"etch-section-style"` in its styles array:

```json
{
  "type": "block",
  "gutenbergBlock": {
    "blockName": "etch/element",
    "attrs": {
      "metadata": { "name": "Hero Section" },
      "tag": "section",
      "attributes": {
        "data-etch-element": "section",
        "class": "hero"
      },
      "styles": ["etch-section-style", "custom-style-id"],
      "script": {
        "code": "base64-encoded-iife"
      }
    },
    "innerBlocks": [...],
    "innerHTML": "...",
    "innerContent": [...]
  },
  "styles": { ... },
  "components": {}
}
```

## JavaScript in Sections

Scripts attach via `attrs.script.code` (base64-encoded):

```javascript
(function(){
'use strict';
// Skip in Etch editor
if(window!==window.top){if(window.frameElement?.id==='etch-iframe')return;}

// Your code here
})();
```

**Script ID format:** `"scr" + 4 alphanumeric chars` (e.g., `"scr1a2b"`)

## Images

Use the figure+img pattern:

```json
{
  "blockName": "etch/element",
  "attrs": {
    "metadata": { "name": "Image" },
    "tag": "figure",
    "attributes": { "class": "hero__image" },
    "styles": ["style-id"]
  },
  "innerBlocks": [{
    "blockName": "etch/element",
    "attrs": {
      "metadata": { "name": "Img" },
      "tag": "img",
      "attributes": {
        "src": "https://example.com/image.jpg",
        "alt": "Description",
        "loading": "lazy"
      },
      "styles": []
    },
    "innerBlocks": [],
    "innerHTML": "",
    "innerContent": []
  }],
  "innerHTML": "\n\n",
  "innerContent": ["\n", null, "\n"]
}
```
