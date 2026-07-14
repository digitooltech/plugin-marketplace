---
name: outreach-sequence
description: >
  Writes a multi-step sales or marketing email sequence (cold outreach, nurture, or follow-up
  cadence) from a product, offer, or campaign brief. Use this skill when the user wants a series
  of emails, a drip campaign, a nurture flow, a sales cadence, or follow-up messages. Strong
  signals: "email sequence", "cold outreach", "drip", "nurture", "cadence", "follow-up emails",
  "SDR emails". This skill writes the actual emails; it starts from a message or brief and does
  NOT define overall campaign strategy (use campaign-brief for that).
---

# Outreach Sequence Skill

Writes a ready-to-send email sequence from a brief, offer, or core message.

---

## Step 1: Lock the inputs

Confirm or infer, and mark inferences:

| Field | Notes |
|---|---|
| **Audience** | Who receives these (segment, role, warmth) |
| **Offer / message** | What the sequence is driving toward |
| **Sequence type** | Cold outreach, nurture, re-engagement, follow-up |
| **Desired action** | The one CTA every email supports |
| **Length** | Number of emails (default 4 if unspecified) |

---

## Step 2: Design the arc

Each email has one job. A typical 4-step arc:

1. **Hook** - relevance and a reason to care. No hard pitch.
2. **Value** - a concrete benefit or proof point tied to their pain.
3. **Proof** - social proof, result, or specific example.
4. **Direct ask** - clear CTA, low friction, easy to say yes to.

Vary the angle per email. Never resend the same pitch louder.

---

## Step 3: Write each email

For every step provide:

- **Send timing** (for example: Day 0, Day 3, Day 7).
- **Subject line** plus one alternate to A/B test.
- **Body**: short, skimmable, one idea, one CTA. Write in the brand voice if given, plain and direct otherwise.
- **CTA**: the specific action and link placeholder.

Keep bodies tight. Cut any sentence that does not move the reader toward the CTA.

---

## Output template

```markdown
# Outreach Sequence: [Name]

**Audience:** ... | **Goal / CTA:** ... | **Type:** ...

### Email 1 - [Day 0] - [purpose]
- **Subject:** ... _(alt: ...)_
- **Body:**
  ...
- **CTA:** ...

### Email 2 - [Day 3] - [purpose]
...

## Notes for review
- [Any claim, number, or promise that needs verification before send]
```

---

## Output rules

- Render in chat as Markdown. No file unless asked.
- Placeholders in `[brackets]` for anything you do not know (names, links, numbers). Never invent specifics like fake customer names or stats.
- Flag every factual claim so `copy-reviewer` can verify it.
- This skill writes emails only. For social posts use `social-post-pack`; for overall strategy use `campaign-brief`.
