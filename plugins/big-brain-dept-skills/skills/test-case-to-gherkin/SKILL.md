---
name: test-case-to-gherkin
description: >
  Converts existing test cases, plain-English test steps, or Given/When/Then acceptance
  criteria into Gherkin `.feature` files. Use this skill when the user provides test cases
  or steps that ALREADY EXIST and asks for Gherkin, feature files, BDD scenarios, Cucumber,
  SpecFlow, or "automation-ready" scenarios. Strong signals: pasted step/action/expected
  tables, numbered test steps, or acceptance criteria written as Given/When/Then. This skill
  starts from test material that exists; it does NOT generate test coverage from a raw ticket.
---

# Test Case to Gherkin Skill

Converts test cases or acceptance criteria that already exist into clean Gherkin `.feature` files.

---

## Step 1: Identify the source material

Read the input and confirm it is one of:

- A test-case table (Step / Action / Expected Result).
- Numbered or bulleted plain-English test steps.
- Acceptance criteria written in Given/When/Then form.

If the input is a **raw Jira ticket with no test cases yet**, stop and hand off — that is `jira-to-test-plan`'s job. This skill only transforms material that already describes concrete test behaviour.

---

## Step 2: Map each case to a scenario

For every test case or criterion, produce one `Scenario`:

- Preconditions → `Given` steps.
- The tester's actions → `When` steps.
- Expected results → `Then` steps.
- Use `And` / `But` for additional clauses.
- Where several cases differ only by input values, collapse them into a single `Scenario Outline` with an `Examples:` table.

Keep steps declarative and reusable. Prefer "When the user submits the login form" over UI-coordinate detail.

---

## Step 3: Emit the feature file

```gherkin
Feature: [Short feature name derived from the source]
  [One-line description of the behaviour under test.]

  Background:
    Given [shared precondition, if every scenario needs it]

  Scenario: [Happy path name]
    Given [precondition]
    When [action]
    Then [observable expected result]

  Scenario Outline: [Parameterised case name]
    Given [precondition]
    When [action with <param>]
    Then [expected <result>]

    Examples:
      | param | result |
      | ...   | ...    |
```

---

## Output rules

- Output valid Gherkin in a fenced ```gherkin block. Render in chat; do not write a file unless the user asks.
- One `Feature` per logical area. If the input spans unrelated areas, emit multiple features separated by `---`.
- Every `Then` must assert an observable, verifiable outcome — never "works correctly".
- If a source case is too vague to turn into a concrete `Then`, keep the scenario but mark the gap with a `# TODO:` comment on that line rather than inventing behaviour.
