---
name: bug-report-triage
description: >
  Triages a bug report or defect: assigns a severity and priority, extracts clean numbered
  reproduction steps, identifies the likely affected area, and flags missing information.
  Use this skill when the user pastes a bug report, defect, incident, or crash and wants it
  triaged, prioritised, or turned into actionable repro steps. Strong signals: "expected vs
  actual", stack traces, error messages, "it crashes / breaks / fails", or a Jira BUG-type
  ticket. This skill assesses and reproduces a defect; it does NOT design test coverage.
---

# Bug Report Triage Skill

Turns a raw bug report into a triaged, actionable defect record.

---

## Step 1: Extract the facts

Pull out whatever is present:

| Field | Notes |
|---|---|
| **Title / summary** | One line describing the defect |
| **Environment** | Build, OS, browser, tenant, etc. if stated |
| **Steps observed** | The sequence that led to the problem |
| **Expected behaviour** | What should have happened |
| **Actual behaviour** | What actually happened, including any error text or stack trace |

If **expected vs actual** cannot be determined, that is the single most important gap — flag it first.

---

## Step 2: Assign severity and priority

Assess independently and justify each in one clause.

- **Severity** (impact): `Blocker` (data loss / crash / no workaround), `Major` (core function broken, workaround exists), `Minor` (limited or cosmetic), `Trivial`.
- **Priority** (urgency to fix): `P1`–`P4`, informed by severity plus reach (how many users / how often).

Never assign a level without a stated reason. If impact is unknowable from the report, say so and give a conservative estimate.

---

## Step 3: Produce clean reproduction steps

Rewrite whatever the reporter gave into an unambiguous numbered list:

1. Precondition / starting state.
2. Concrete action ("Click **Save**"), not "save the thing".
3. ...
4. **Observed:** what goes wrong. **Expected:** what should happen instead.

If the report lacks enough detail to reproduce, list the exact questions that would unblock reproduction.

---

## Output template

```markdown
# Bug Triage: [Title]

| Field | Value |
|---|---|
| Severity | [level] — [reason] |
| Priority | [P#] — [reason] |
| Affected area | [component / feature, inferred if not stated] |

## Reproduction steps
1. ...
2. ...
   - **Observed:** ...
   - **Expected:** ...

## Missing information
- [ ] [Anything needed to confirm or fix, if applicable]
```

---

## Output rules

- Render in chat as Markdown; no file unless asked.
- Mark every inference with `> ⚠️ Assumed: ...`. Do not state a severity, priority, or affected area as fact when it was inferred.
- This skill triages one defect. If the user actually wants test cases or a QA plan for the area, that is `jira-to-test-plan`.
