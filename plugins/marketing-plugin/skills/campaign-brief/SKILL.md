---
name: campaign-brief
description: >
  Turns a raw product, launch, or campaign idea into a structured go-to-market campaign brief:
  audience, positioning, core message, channels, goals, and success metrics.
  Use this skill when the user describes a product launch, a marketing campaign, a promotion,
  or a go-to-market effort and wants it shaped into a plan. Strong signals: "we're launching",
  "GTM", "go-to-market", "campaign", "positioning", "messaging". This skill produces the
  strategic brief that the outreach-sequence and social-post-pack skills then build assets from.
  It does NOT write the emails or social posts itself.
---

# Campaign Brief Skill

Turns a rough launch or campaign idea into a brief the rest of the team can build assets from.

If a target audience has not been researched yet, prefer the `audience-researcher` agent first,
then fold its findings into Step 1. Do not block on it: if the user already gave you an audience,
proceed.

---

## Step 1: Establish the foundation

Pull out or infer the following. Mark anything you inferred.

| Field | Notes |
|---|---|
| **Product / offer** | What is being marketed, in one sentence |
| **Target audience** | Who it is for (ICP, segment, persona) |
| **Primary goal** | Awareness, leads, signups, revenue, retention |
| **Timeline** | Launch date or campaign window if stated |
| **Constraints** | Budget, channels, brand, compliance if stated |

---

## Step 2: Positioning and message

- **Positioning statement**: For [audience] who [need], [product] is a [category] that [key benefit], unlike [alternative].
- **Core message**: one sentence the whole campaign ladders up to.
- **Message pillars**: 3 supporting themes, each a benefit backed by a proof point.

Keep claims defensible. If a proof point is assumed, flag it so the copy-reviewer can check it.

---

## Step 3: Channels and plan

- Recommend 2 to 4 channels matched to where the audience actually is, with a one-line reason each.
- Note the role of each channel (reach, nurture, convert).
- Flag the assets each channel needs (for example: outreach email sequence, LinkedIn posts, landing page).

---

## Step 4: Goals and metrics

- Turn the primary goal into 1 to 3 measurable targets (leading + lagging).
- State the metric, a target if one can be reasonably set, and how it is measured.

---

## Output template

```markdown
# Campaign Brief: [Name]

## Foundation
| Field | Value |
|---|---|
| Product | ... |
| Audience | ... |
| Goal | ... |
| Timeline | ... |

## Positioning
- **Statement:** ...
- **Core message:** ...
- **Pillars:**
  1. ... (proof: ...)
  2. ...
  3. ...

## Channel plan
- **[Channel]** - [role] - [assets needed]

## Success metrics
- [Metric] - [target] - [how measured]

## Recommended next steps
- [ ] [Which assets to build next, e.g. outreach-sequence, social-post-pack]
```

---

## Output rules

- Render in chat as Markdown. No file unless asked.
- Mark every inference with `> Assumed: ...`. Do not present an inferred audience or metric as fact.
- This skill stops at the brief. When the user is ready for assets, hand off to `outreach-sequence`
  (sales/marketing emails) or `social-post-pack` (social content), and to `copy-reviewer` for a check.
