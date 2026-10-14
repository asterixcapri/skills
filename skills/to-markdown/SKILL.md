---
name: to-markdown
description: Save the current discussion or a user-specified topic as a standalone Markdown document, using show-me when a visual explanation helps. Use when the user wants a conversation, explanation, comparison, or decision captured in a Markdown file.
---

# To Markdown

Produce a Markdown file a reader can understand without the conversation.
Preserve the user's language unless the destination's instructions require
another language. Follow repository instructions and established document formats.

## Establish the document

Use the requested topic and audience, or the current discussion when omitted.
Capture the relevant context, explanation, decisions, tradeoffs, and unresolved
questions. Distinguish agreed decisions from proposals; preserve uncertainty.
Read referenced code or documents when needed to support concrete claims.
Choose the structure around the topic rather than transcribing the chat.

Use the requested path. Otherwise follow an established documentation location,
falling back to `docs/explanations/<topic>.md` in the working directory. Choose
a short kebab-case filename and create parent directories. Read an existing
target before editing it; preserve unrelated content and use a fresh filename
when replacement was not requested.

## Decide whether a visual helps

Use a visual when relationships, ordering, structure, logic, or a before/after
comparison become easier to understand. Prose and tables can be enough.

When a visual helps, check that `show-me` is available and read its `SKILL.md`
before designing the visual. If unavailable, stop before the visual step and
give the exact installation command:

```bash
npx skills@latest add humanlayer/skills --skill show-me
```

Topic selection, evidence gathering, and a prose outline remain runnable.
Keep any partial draft clearly marked as incomplete.

Apply `show-me`'s representation choices within this Markdown output contract:
use fenced Mermaid for relationships and sequences, fenced text for trees or
pseudocode, and fenced code or diffs for concrete changes. Place each visual
next to the explanation it supports. Plain-text documents need no `show-me`.

## Write and verify

Write the file with a descriptive title, enough context to stand alone, and
only the sections the subject needs. Link sources beside the claims they
support. Use readable labels and explain the main point of each diagram in
text so the document also makes sense in viewers without Mermaid support.

Read the saved file back. Check that code fences are balanced, links and paths
are appropriate for the destination, and decisions and open questions match
the discussion. If a Mermaid renderer is available, validate the diagrams;
otherwise report that they were not rendered. Return a clickable file link
and any material verification limits.

## Attribution

Visual guidance comes from [Dex Horthy's `show-me`](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md),
used as a conditional runtime dependency.
