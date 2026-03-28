---
name: brief
description: Gather business info, target audience, offer details, and funnel strategy to create a project brief
user_invocable: true
---

# Create Funnel Brief

Gather all the information needed to build a high-converting landing page and funnel.

## Instructions

Ask the user these questions conversationally (not as a form). Skip anything they've already mentioned.

### Business Context
1. What's the business / brand name?
2. What industry or niche? (fitness, SaaS, coaching, ecommerce, etc.)
3. What's the main product or service being offered?
4. What's the price point? (or is it a free trial / lead magnet?)
5. Who are the main competitors?

### Target Audience
6. Who is the ideal customer? (age, gender, location, interests)
7. What problem does the customer have right now?
8. What have they already tried that didn't work?
9. What would their life look like after using this product?

### Offer & Funnel
10. What's the specific offer on this page? (free trial, discount, consultation, etc.)
11. What happens after they sign up? (app access, call booking, email sequence, etc.)
12. Is there a payment involved? If so, when and how much?
13. What's the desired funnel flow? (single page, multi-step, VSL + form, etc.)

### Brand & Tone
14. What's the brand personality? (premium, casual, energetic, clinical, etc.)
15. Any specific words or phrases the brand uses?
16. Are there existing brand assets? (logo, colors, fonts, photos)

### Technical
17. Does the WordPress site already exist?
18. Is n8n already set up?
19. Any existing CRM or customer database?
20. What tracking is already in place?

## Output

Create `brief.md` in the project folder with structured sections:

```markdown
# Project Brief: [Name]

## Business
...

## Target Audience
...

## Offer
...

## Funnel Flow
...

## Brand & Tone
...

## Technical Status
...

## Key Messages
- Main headline direction: ...
- Core promise: ...
- Primary objection to overcome: ...
- Social proof available: ...
```

This brief feeds into `/funnel-builder:copy` and `/funnel-builder:design`.
