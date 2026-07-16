---
name: response-draft
description: >
  Drafts a clear, on-brand reply to a customer support ticket: acknowledges the issue, answers or
  sets expectations, and gives a concrete next step. Use this skill when the user wants to reply to
  a customer, write a support response, or answer a ticket. Strong signals: "reply to this",
  "draft a response", "answer the customer", "write back". This skill writes the reply; it does not
  classify or route the ticket (use ticket-triage for that).
---

# Response Draft Skill

Writes a support reply that is warm, clear, and specific.

If the answer likely lives in the knowledge base, prefer the `kb-searcher` agent first so the reply can cite it.
Do not block on it: if you already know the answer, proceed.

---

## Step 1: Read the situation

- What did the customer ask, and what is their emotional state (frustrated, confused, neutral)?
- Do you actually have the answer, or do you need to set an expectation and follow up?
- What is the single next step that moves this forward?

---

## Step 2: Draft the reply

Structure every reply as:

1. **Acknowledge** - name the issue and, if warranted, the frustration. One line.
2. **Answer or set expectations** - the fix, the steps, or an honest "here's what happens next".
3. **Next step** - one concrete action, with a timeframe if you can give one.
4. **Close** - a short, human sign-off.

Match the tone to the sentiment: more warmth and ownership when the customer is upset, more brevity when they just want an answer.

---

## Step 3: Check before sending

- No blame on the customer, no jargon they would not know.
- Every claim is one you can stand behind; flag any promise that needs a real owner.
- Keep it as short as it can be while still being complete.

---

## Output template

```markdown
**Subject:** [if email]

Hi [name],

[Acknowledge]

[Answer / expectation]

[Next step, with timeframe]

[Sign-off],
[Agent / team]
```

---

## Output rules

- Render in chat as Markdown. No file unless asked.
- Offer one draft, then adjust on request. Do not send anything; this only drafts.
- Flag any commitment (refund, ETA, escalation) that needs a real human to own with `> Needs owner: ...`.
