---
name: incident-report
description: >
  Turns a rough incident description or a set of alerts, logs, and timestamps into a structured
  incident report: summary, impact, timeline, root cause, and follow-up actions.
  Use this skill when the user describes an outage, a production incident, a postmortem, or wants
  to write up what happened. Strong signals: "incident", "outage", "postmortem", "went down",
  "root cause", "RCA". This skill produces the write-up; it does not diagnose logs itself.
  Prefer the log-analyzer agent first when raw logs need to be read.
---

# Incident Report Skill

Turns a messy account of an incident into a report the team can review and learn from.

If raw logs still need reading, prefer the `log-analyzer` agent first, then fold its findings into the timeline.
Do not block on it: if the user already gave you a timeline, proceed.

---

## Step 1: Establish the facts

Pull out or infer the following. Mark anything you inferred.

| Field | Notes |
|---|---|
| **What broke** | The user-facing symptom in one sentence |
| **Impact** | Who and what was affected, and for how long |
| **Detection** | How it was noticed (alert, customer report, dashboard) |
| **Severity** | Sev1-Sev4 if stated or inferable |

---

## Step 2: Build the timeline

List events in order, each with a timestamp and a one-line description.
Cover detection, escalation, mitigation, and resolution.
Mark any timestamp you had to estimate.

---

## Step 3: Root cause and contributing factors

- State the **root cause** in one or two sentences, backed by evidence from the timeline.
- List **contributing factors** that made it worse or slower to resolve.
- Keep it blameless: describe systems and gaps, not people.

---

## Step 4: Follow-up actions

Turn the analysis into 2 to 5 concrete action items.
Each has an owner placeholder, a clear outcome, and a rough priority.

---

## Output template

```markdown
# Incident Report: [Title]

## Summary
[2-3 lines: what happened, impact, resolution]

## Impact
- **Affected:** ...
- **Duration:** ...
- **Severity:** ...

## Timeline
- `HH:MM` - ...
- `HH:MM` - ...

## Root cause
[1-2 sentences, evidence-backed]

## Contributing factors
- ...

## Follow-up actions
- [ ] [Owner] - [outcome] - [priority]
```

---

## Output rules

- Render in chat as Markdown. No file unless asked.
- Mark every inference with `> Assumed: ...`. Do not present an estimated timestamp or root cause as confirmed fact.
- Stay blameless. Attribute problems to systems and process gaps, never individuals.
