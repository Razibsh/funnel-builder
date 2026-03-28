---
name: copy
description: Generate landing page copy — headlines, subheadlines, CTAs, form labels, objection handlers, trust signals, and micro-copy
user_invocable: true
---

# Generate Landing Page Copy

Create conversion-optimized copy for all sections of a landing page or funnel.

## Instructions

1. Read `funnel-config.json` for language and direction
2. If a `brief.md` exists, use it for business context
3. If no brief exists, ask for:
   - Business type / industry
   - Target audience (age, gender, pain points)
   - Main offer (what they get)
   - Price point (or "free trial")
   - Key differentiator (why this vs competitors)
   - Desired tone (professional, casual, urgent, premium)
4. Generate copy for all requested sections

## Copy Frameworks

### AIDA (Attention → Interest → Desire → Action)
Best for: Hero sections, email sequences

### PAS (Problem → Agitate → Solution)
Best for: Long-form sales pages, VSL scripts

### BAB (Before → After → Bridge)
Best for: Testimonial sections, case studies

## Section Copy Templates

### Hero
- **Headline** (5-10 words, benefit-driven)
- **Subheadline** (1-2 sentences, expand on promise)
- **CTA button** (2-4 words, action-oriented)
- **Social proof line** (e.g., "Join 2,000+ members")

### Features
- **Section title**
- Per feature: **Icon suggestion**, **Title** (3-5 words), **Description** (1-2 sentences)

### Signup Form
- **Section headline** (motivate action)
- **Section subtitle**
- Per step: **Step title**, **Field labels**, **Button text**
- **Terms text**
- **Security/trust text**
- **Error messages** (validation, payment, general)
- **Success message** (title + description)
- **Loading messages** (rotating, 3-4 options)

### Pricing
- **Price display** (format: ₪XX / month)
- **Price note** (e.g., "Cancel anytime", "First 30 days free")
- **What's included** (bullet list)
- **Guarantee text**

### FAQ
- 5-8 questions with answers covering:
  - How it works
  - Pricing / cancellation
  - What's included
  - Who it's for
  - Technical requirements

### Final CTA
- **Headline** (urgency or summary)
- **Subtext** (risk reversal)
- **CTA button** (same as hero or stronger)

## Language Guidelines

### Hebrew (RTL)
- Keep it conversational, direct
- Use פנייה ישירה (direct address — "אתה/את")
- Short sentences, minimal jargon
- Emojis work well for trust badges and step indicators
- Price format: XX ₪ (number first, then shekel sign)

### English (LTR)
- Active voice, present tense
- "You" focused (not "we")
- Power words: free, proven, guaranteed, instant, exclusive

## Output Format

Output structured copy as markdown with clear section headers. Include alternate versions for A/B testing where relevant.

Save as `copy/{page-name}.md` in the project folder.
