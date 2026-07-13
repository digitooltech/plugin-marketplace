# Orchestration rules — big-brain-dept-skills

These are hard constraints for skill routing in this plugin.
Follow them for the whole session. They override your default judgement about when to act.

This plugin ships three skills:

- `jira-to-test-plan` — raw Jira ticket → structured test plan / test cases (+ Gantt for Tasks).
- `test-case-to-gherkin` — existing test cases or Given/When/Then criteria → Gherkin `.feature` files.
- `bug-report-triage` — bug report / defect → severity, priority, and clean reproduction steps.

Their triggers overlap on purpose. The rules below decide which one fires.

## Routing table

| Situation | Skill |
|---|---|
| A **raw Jira ticket** is pasted (Summary, Description, Acceptance Criteria, key like `PROJ-123`) and the user wants test coverage | `jira-to-test-plan` |
| User asks to "generate a test plan", "write test cases", or "create a QA plan" from a ticket or requirement | `jira-to-test-plan` |
| User provides **test cases or steps that already exist** (Step/Action/Expected tables, numbered steps, or Given/When/Then) and wants Gherkin, feature files, BDD, Cucumber, or SpecFlow | `test-case-to-gherkin` |
| User pastes a **bug/defect** (expected-vs-actual, stack trace, error text, "it crashes/fails") and wants triage, severity, priority, or reproduction steps | `bug-report-triage` |
| Prose with no ticket, no test material, and no defect | None of the above — do not force a skill. |

## Disambiguation

Resolve these by the **form of the input** and the **user's stated intent**, in that order.

- **`jira-to-test-plan` vs `test-case-to-gherkin`.** The deciding question is *does test material already exist?* A raw ticket (even one with Given/When/Then acceptance criteria) → `jira-to-test-plan`: it must first design the cases. A block of already-written test cases or steps → `test-case-to-gherkin`: it only reformats. If a ticket contains full ready-to-run test cases and the user explicitly asks for Gherkin, treat the cases as existing material and use `test-case-to-gherkin`.
- **`jira-to-test-plan` vs `bug-report-triage`.** A Jira **bug** ticket matches both. Decide by intent: "test plan / test cases / QA coverage" → `jira-to-test-plan`; "triage / severity / priority / how do I reproduce this" → `bug-report-triage`. If intent is unstated for a bug ticket, ask which the user wants before generating — do not assume.
- **`bug-report-triage` vs `test-case-to-gherkin`.** A defect is not test material. Never route a bug report to `test-case-to-gherkin`. Triage it first; the resulting repro steps can *then* be fed to `test-case-to-gherkin` if the user asks for a regression scenario.

## Precedence rules

When more than one skill still matches after disambiguation:

1. **Explicit output format wins.** If the user names a concrete artefact — "Gherkin", "feature file", "triage", "severity", "test plan" — route to the skill that produces it, over any skill matched only by input shape.
2. **Bug intent beats test-plan intent on a defect.** For input that is unmistakably a defect (stack trace / crash / expected-vs-actual), `bug-report-triage` takes precedence over `jira-to-test-plan` unless the user explicitly asked for test cases.
3. **Design before transform.** `jira-to-test-plan` precedes `test-case-to-gherkin` whenever the test cases do not yet exist. Do not skip straight to Gherkin from a bare ticket.
4. **Never run two skills silently.** Chaining (triage → Gherkin, or test-plan → Gherkin) happens only when the user asks for the second step. Do the first skill, then offer the next.

## Cross-cutting constraints

These apply to every skill in this plugin.

- **Observable expected results.** Every expected result, `Then` clause, or repro outcome must be verifiable. Reject "works correctly".
- **Mark every inference.** Flag assumed content inline: `> ⚠️ Assumed: ...` for Markdown skills, `# TODO:` for Gherkin. Never present inferred severity, ticket fields, or behaviour as fact.
- **Render in chat, no files by default.** Output Markdown or Gherkin inline. Write a file only when the user asks.
- **No preamble.** Start directly with the artefact's heading or fenced block — not "Here is your...".
- **Never hand-write chart code.** The Gantt chart in `jira-to-test-plan` is produced only by its bundled `scripts/generate_gantt.py`. Do not draw or code charts yourself.
- **Multiple items in one paste.** If the input contains several tickets, defects, or case sets, process each separately and separate outputs with `---`.

## Open questions

Unresolved because the skills do not encode an answer. Surface to the user when relevant; do not guess.

1. **Gantt output location.** `jira-to-test-plan` writes chart inputs and output to `/tmp`. Whether that path is valid in Cowork's execution sandbox, or should be session-scoped, is not specified.
2. **Chaining ownership.** When a user wants triage → regression Gherkin in one request, no skill owns the hand-off contract (what fields pass from `bug-report-triage` to `test-case-to-gherkin`). Confirm the intermediate with the user until this is defined.
