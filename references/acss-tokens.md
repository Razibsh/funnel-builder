# Automatic.css (ACSS) Token Reference

ACSS provides design tokens as CSS custom properties. This reference covers all available tokens for use in Etch JSON patterns.

## Color System

### Main Color Roles (6 roles)

| Role | Variable | Purpose |
|------|----------|---------|
| Primary | `var(--primary)` | Main brand / CTA |
| Secondary | `var(--secondary)` | Secondary brand |
| Tertiary | `var(--tertiary)` | Third brand color |
| Accent | `var(--accent)` | Accent highlights |
| Base | `var(--base)` | Backgrounds / body text |
| Neutral | `var(--neutral)` | Greys |

### Shade Scale (for each color {name})

```
var(--{name})               /* Main color */
var(--{name}-hover)         /* Hover state */
var(--{name}-ultra-light)   /* ~95% lightness */
var(--{name}-light)         /* ~85% lightness */
var(--{name}-semi-light)
var(--{name}-semi-dark)
var(--{name}-dark)          /* ~25% lightness */
var(--{name}-ultra-dark)    /* ~10% lightness */
```

### Transparency (ACSS 4.x)

Old `--{name}-trans-10` tokens are REMOVED. Use `color-mix()`:

```css
background: color-mix(in oklch, var(--primary) 20%, transparent);
```

### Semantic Colors

`var(--warning)`, `var(--info)`, `var(--success)`, `var(--danger)` — each with full shade scale + hover.

### Utility Colors

`var(--white)`, `var(--black)`

## Typography

### Heading Sizes (fluid by default)

```
var(--h1)    /* ~3rem fluid */
var(--h2)    /* ~2.25rem fluid */
var(--h3)    /* ~1.5rem fluid */
var(--h4)    /* ~1.25rem fluid */
var(--h5)    /* ~1.125rem fluid */
var(--h6)    /* ~1rem fluid */
```

### Text Sizes

```
var(--text-xs)   /* ~0.75rem */
var(--text-s)    /* ~0.875rem */
var(--text-m)    /* ~1rem (base) */
var(--text-l)    /* ~1.125rem */
var(--text-xl)   /* ~1.25rem */
var(--text-xxl)  /* ~1.5rem */
```

### Font Families

```
var(--heading-font-family)   /* e.g., Rubik, sans-serif */
var(--body-font-family)      /* e.g., Noto Sans Hebrew, sans-serif */
```

### Line Heights & Weights

```
var(--body-line-height)     /* Typically 1.65 */
var(--heading-line-height)  /* Typically 1.2 */
var(--heading-font-weight)  /* Typically 700-900 */
```

## Spacing

### Content Spacing

```
var(--space-xs)    /* ~0.5rem */
var(--space-s)     /* ~0.75rem */
var(--space-m)     /* ~1rem */
var(--space-l)     /* ~1.5rem */
var(--space-xl)    /* ~2.5rem */
var(--space-xxl)   /* ~3rem */
```

### Section Spacing (larger, for section padding)

```
var(--section-space-xs)
var(--section-space-s)
var(--section-space-m)
var(--section-space-l)   /* ~5rem */
var(--section-space-xl)
var(--section-space-xxl)
```

### Gutter

```
var(--gutter)    /* Inline padding for content areas, ~1.5rem */
```

## Layout

```
var(--content-width)      /* Max content width */
var(--container-width)    /* Max container width */
var(--narrow-width)       /* Narrow content width */
var(--grid-gap)           /* Default grid gap */
```

## Border & Radius

```
var(--radius)       /* Base radius (e.g., 5px) */
var(--radius-xs)    /* Extra small */
var(--radius-s)     /* Small */
var(--radius-l)     /* Large */
var(--radius-xl)    /* Extra large */
var(--radius-xxl)   /* Extra extra large */
var(--radius-50)    /* 50% (circle) */
var(--border-size)  /* Default border width (1px) */
```

**Note:** Extra radius sizes require "Generate Extra Radius Sizes" to be ON in ACSS settings.

## Buttons

```
var(--btn-padding-block)
var(--btn-padding-inline)
var(--btn-radius)
var(--btn-font-size)
var(--btn-font-weight)
```

## Transitions

```
var(--transition)    /* Default: 0.3s ease-in-out */
```

## Always Use Fallbacks

ACSS tokens may not be available in all WordPress contexts. Always provide fallbacks:

```css
/* Correct: */
color: var(--primary, #edad24);
padding: var(--space-m, 1rem);
font-family: var(--heading-font-family, Rubik, sans-serif);

/* Incorrect — will fail silently: */
color: var(--primary);
```

## Dark Theme Pattern

```css
/* Section background */
background: var(--base, #110a00);

/* Card background */
background: color-mix(in oklch, var(--white, #fff) 3%, transparent);

/* Card border */
border: 1px solid color-mix(in oklch, var(--white, #fff) 8%, transparent);

/* Subtle text */
color: var(--neutral-semi-light, #a6a6a6);

/* Muted text */
color: var(--neutral-semi-dark, #595959);

/* Input background */
background: color-mix(in oklch, var(--white, #fff) 4%, transparent);

/* Input border */
border: 1px solid color-mix(in oklch, var(--white, #fff) 10%, transparent);

/* Input focus */
border-color: var(--primary, #edad24);
```

## Light Theme Pattern

```css
/* Section background */
background: var(--white, #fff);

/* Card background */
background: var(--base-ultra-light, #f8f8f8);

/* Card border */
border: 1px solid var(--neutral-ultra-light, #e5e5e5);

/* Body text */
color: var(--base, #1a1a1a);

/* Heading text */
color: var(--base-dark, #111);

/* Subtle text */
color: var(--neutral-semi-dark, #666);
```
