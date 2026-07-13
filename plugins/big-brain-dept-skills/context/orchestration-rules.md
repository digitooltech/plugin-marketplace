# Orchestration rules — big-brain-dept-skills

These are hard constraints for skill routing in this plugin.
Follow them for the whole session. They override your default judgement about when to act.

This plugin currently ships exactly one skill: `jira-to-test-plan`.
There is no second skill to route against inside this plugin, so most routing decisions here are about *internal branches* of that one skill and about *collisions with identically-named skills in other plugins*.

## Routing table

| Situation | Action |
|---|---|
| User pastes or shares Jira-style content (fields like Summary, Description, Acceptance Criteria, or a ticket key such as `PROJ-123`) | Invoke `jira-to-test-plan`. Do not wait for the word "skill". |
| User asks to "generate a test plan", "write test cases", or "create a QA plan" from a ticket or requirement | Invoke `jira-to-test-plan`. |
| Ticket type is **Epic** | Produce Template A (high-level test plan). Do **not** generate a Gantt chart. |
| Ticket type is **Task** or **Sub-task** | Produce Template B (step-by-step test cases), then always generate the Gantt chart via the bundled script. |
| Ticket type is unclear | Default to Template B and state in the output that the type was assumed. |
| User pastes multiple tickets at once | Generate one plan per ticket, separated by `---`. Apply the per-ticket type routing above to each. |
| Content is prose with no ticket structure and no request for a test plan | Do **not** invoke `jira-to-test-plan`. This skill converts tickets; it is not a general writing tool. |

## Disambiguation

- **Test plan vs. general Markdown request.** `jira-to-test-plan` fires only when the input is a Jira ticket or an explicit test-plan/test-case/QA request. A request to summarize, reformat, or write arbitrary Markdown is not a trigger.
- **Epic vs. Task/Sub-task branch.** These are mutually exclusive within a single ticket. Decide once per ticket by the stated type; infer from content only when the type is absent. The Gantt chart follows the branch: Task/Sub-task always, Epic never.

## Precedence rules

- Inside this plugin there is no skill-vs-skill contention, so no intra-plugin precedence applies.
- **Cross-plugin collision:** `dept-comms-skills` and `new-dept` each ship a skill with the same `name` (`jira-to-test-plan`) and a byte-identical description. On a pasted Jira ticket, all three are equally strong candidates and nothing in their metadata breaks the tie. Do **not** silently pick this plugin's copy. If more than one `jira-to-test-plan` skill is available in the session, ask the user which department's skill to use before proceeding. See open question 1.

## Cross-cutting constraints

These apply to every `jira-to-test-plan` invocation regardless of branch.

- **Parse before generating.** Extract ticket key, type, summary, description, and acceptance criteria first. If a critical field (Summary, Description) is missing or ambiguous, make a reasonable inference and mark it inline with a `> ⚠️ Assumed: ...` blockquote. Never silently invent ticket content.
- **AC traceability.** For Tasks, trace every acceptance criterion to at least one test case. Flag any criterion that cannot be tested as written.
- **Observable expected results.** Every expected result must be verifiable. Reject vague outcomes like "works correctly".
- **Never hand-write chart code.** Produce the Gantt chart only by running the bundled `scripts/generate_gantt.py` against a `/tmp/test_cases.json` file built to the schema in the skill. Do not draw or code the chart yourself.
- **Output format.** Render the plan as Markdown in chat. Start directly with the `# Test Plan:` or `# Test Cases:` heading — no preamble. Do not write a file unless the user asks.
- **Thin tickets.** If a ticket is very short or underspecified, generate what you can and flag every gap with `> ⚠️` blockquotes rather than padding with assumptions.

## Open questions

These are unresolved because the current skills do not encode an answer. Do not guess a resolution — surface them to the user when they become relevant.

1. **Which `jira-to-test-plan` wins across plugins?** The three department copies are indistinguishable by name and description. There is no signal (project, label, department context) in the skills that selects one. Until an owner defines a tiebreaker, ask the user which one to run.
2. **Gantt output location.** The skill writes chart inputs and output to `/tmp`. Whether that is acceptable in Cowork's execution sandbox, or should be a session-scoped path, is not specified by the skill.
