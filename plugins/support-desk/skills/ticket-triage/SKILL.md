---
name: ticket-triage
description: >
  Turns an incoming support ticket or customer message into a structured triage: category,
  priority, sentiment, and a recommended next action. Use this skill when the user pastes a support
  ticket, a customer email, or a bug report and wants it sorted or routed. Strong signals: "triage",
  "support ticket", "route this", "how urgent", "categorize". This skill classifies and routes; it
  does not write the customer reply (use response-draft for that).
---

# Ticket Triage Skill

Turns a raw customer message into a routing decision the support team can act on.

If the tone is hard to read, prefer the `sentiment-analyzer` agent first, then fold its read into the triage.
Do not block on it: if the sentiment is obvious, proceed.

---

## Step 1: Extract the request

Pull out or infer the following. Mark anything you inferred.

| Field | Notes |
|---|---|
| **Ask** | What the customer actually wants, in one sentence |
| **Product area** | The feature or component involved |
| **Type** | Bug, how-to, billing, feature request, outage |
| **Blocked?** | Whether the customer is fully blocked or has a workaround |

---

## Step 2: Assign priority

Set a priority from the impact and urgency:

- **P1** - fully blocked, data loss, security, or many users affected.
- **P2** - significant impact with a workaround, or a single important customer.
- **P3** - minor issue, question, or cosmetic.
- **P4** - feature request or nice-to-have.

State the one factor that drove the priority.

---

## Step 3: Route and recommend

- Recommend the **owning team or queue** (e.g. billing, engineering, success).
- Suggest the **next action**: reply with known answer, escalate, ask for logs, file a bug.
- If a knowledge-base article likely answers it, note that the `kb-searcher` agent can find it.

---

## Output template

```markdown
# Triage: [Short title]

## Request
[1 sentence]

## Classification
| Field | Value |
|---|---|
| Category | ... |
| Priority | P? - [driving factor] |
| Sentiment | ... |
| Blocked | yes / no |

## Recommended route
- **Queue:** ...
- **Next action:** ...
```

---

## Output rules

- Render in chat as Markdown. No file unless asked.
- Mark every inference with `> Assumed: ...`. Do not present an inferred priority as certain.
- Stop at the triage. When a reply is needed, hand off to `response-draft`.
