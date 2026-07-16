---
name: release-notes
description: >
  Turns a raw list of merged changes, commits, or ticket titles into clean, grouped release notes
  written for the people who read them: users, operators, or developers.
  Use this skill when the user wants a changelog, release notes, a "what's new", or a version
  summary. Strong signals: "release notes", "changelog", "what's new", "version bump", "ship notes".
  This skill writes the notes; it does not decide the version number or cut the release.
---

# Release Notes Skill

Turns a pile of commits or ticket titles into notes a human actually wants to read.

---

## Step 1: Gather and classify

Collect the raw changes (commits, PR titles, tickets).
Classify each into one of: **Added**, **Changed**, **Fixed**, **Deprecated**, **Removed**, **Security**.
Drop noise: merge commits, formatting-only changes, internal chores unless they matter to the reader.

---

## Step 2: Rewrite for the audience

Ask who reads this: end users, operators, or developers.
Rewrite each kept change as a benefit- or impact-led line, not a commit message.

- User-facing: lead with what they can now do or what's fixed.
- Operator-facing: call out config, migration, or downtime.
- Developer-facing: note API or behavior changes and breaking changes.

Flag every **breaking change** clearly at the top.

---

## Step 3: Assemble

Group by category, most important first.
Keep each line to one sentence.
Add a short highlights section if the release is large.

---

## Output template

```markdown
# [Version] - [Date]

> Highlights: [1-2 lines, only if the release is large]

## ⚠️ Breaking changes
- ...

## Added
- ...

## Changed
- ...

## Fixed
- ...

## Security
- ...
```

---

## Output rules

- Render in chat as Markdown. No file unless asked.
- Omit any empty category. Do not invent changes that were not in the input.
- If a change's impact is unclear from its title, flag it with `> Needs detail: ...` rather than guessing.
