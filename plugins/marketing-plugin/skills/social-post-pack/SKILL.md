---
name: social-post-pack
description: >
  Generates a pack of social media posts across channels (LinkedIn, X/Twitter, Instagram,
  and similar) from a single announcement, message, or campaign brief. Use this skill when the
  user wants social content, social posts, a content pack, launch posts, or channel-specific
  copy. Strong signals: "social posts", "LinkedIn post", "tweet", "X post", "content pack",
  "social copy", "announcement post". This skill writes channel-ready posts; it does NOT set
  campaign strategy (use campaign-brief) or write email sequences (use outreach-sequence).
---

# Social Post Pack Skill

Turns one core message into channel-ready social posts.

---

## Step 1: Lock the inputs

Confirm or infer, marking inferences:

| Field | Notes |
|---|---|
| **Core message** | The one thing every post communicates |
| **Channels** | Which platforms (default: LinkedIn + X if unspecified) |
| **Audience** | Who the posts speak to |
| **CTA** | The action posts should drive |
| **Voice** | Brand tone if given, otherwise clear and human |

---

## Step 2: Adapt per channel

Write natively for each channel, do not paste the same text everywhere.

- **LinkedIn**: professional, value-led, a hook line + a short body + a soft CTA. Line breaks for skimming.
- **X / Twitter**: one sharp idea under the character limit. Offer a 2 to 3 post thread option if the message needs room.
- **Instagram**: caption-first, visual suggestion in brackets, hashtags grouped at the end.
- Other channels on request, matched to their norms.

---

## Step 3: Package the pack

For each channel provide 1 to 2 post options so the user can pick. Include:

- The post text, ready to paste.
- A bracketed visual or media suggestion.
- Hashtags where the channel expects them (relevant, not stuffed).

---

## Output template

```markdown
# Social Post Pack: [Campaign]

**Core message:** ... | **CTA:** ...

## LinkedIn
**Option A**
...
_[Visual: ...]_

## X / Twitter
**Option A**
...

## Notes for review
- [Any claim or number that needs verification]
```

---

## Output rules

- Render in chat as Markdown. No file unless asked.
- Never invent stats, quotes, or customer names. Use `[brackets]` for anything unknown.
- Respect platform limits (for example, keep X posts within the character count).
- Flag claims for `copy-reviewer`. For emails use `outreach-sequence`; for strategy use `campaign-brief`.
