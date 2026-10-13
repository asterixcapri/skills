---
name: to-html
description: Save the current discussion or a user-specified topic as one self-contained HTML document that works offline, using show-me when a visual explanation helps. Use when the user wants an HTML explanation, comparison, or decision document saved as a file.
---

# To HTML

Produce one HTML file a reader can open locally, offline, and understand without
the conversation. Preserve the user's language unless the destination's
instructions require another language. Follow repository instructions.

## Establish the document

Use the requested topic and audience, or the current discussion when omitted.
Capture the context, explanation, decisions, tradeoffs, and unresolved questions
that matter to the reader. Distinguish decisions from proposals and preserve
uncertainty. Read referenced code or documents when needed to support claims.

Use the requested path. Otherwise follow an established documentation location,
falling back to `docs/explanations/<topic>.html` in the working directory. Choose
a short kebab-case filename and create parent directories. Read an existing
target before editing it; preserve unrelated content and use a fresh filename
when replacement was not requested.

## Decide whether a visual helps

Use a visual when relationships, ordering, structure, logic, or a before/after
comparison become easier to understand. Text and tables can be enough.

When a visual helps, check that `show-me` is available and read its `SKILL.md`
before designing the visual. If unavailable, stop before the visual step and
give the exact installation command:

```bash
npx skills@latest add humanlayer/skills --skill show-me
```

Topic selection, evidence gathering, and a prose outline remain runnable.
Keep any partial draft clearly marked as incomplete. Text-only documents need
no `show-me`. Apply its visual guidance within the output contract below.

## Build from the common template

Read [assets/document.html](assets/document.html) and use it as the starting
point. It supplies typography, spacing, responsive layout, print styles, and
styles for tables, code, and figures. Replace its language, title, introduction,
and content markers. Choose sections to fit the topic; the template is a visual
baseline, not a mandatory report outline. Adapt colors and typography to an
established product identity when relevant, keeping all resources embedded.

Escape text and attribute values for their HTML context; insert only generated
HTML into the content marker. Use semantic HTML and real labels and data. Place visuals beside their short
explanations. Use accessible figures with captions, and preserve readable labels
on narrow screens. Add interaction only when it helps the reader inspect the
subject; the explanation should remain readable without JavaScript.

The delivered file must contain everything it needs to display and function:

- Embed CSS in `<style>` and any JavaScript in `<script>`.
- Use system fonts, inline SVG, and data URIs for raster images.
- Embed libraries if genuinely needed; preserve their license notices.
- Resolve every display dependency inside the file, including resources inside
  SVG and CSS. Use no CDN imports, adjacent asset files, or network requests.
- Keep external source links as optional references, never display dependencies.

### Render Mermaid during generation

Write Mermaid source for graph-shaped relationships or sequences. Render it to
SVG during generation and embed the resulting `<svg>` in the HTML. The final
page loads no Mermaid runtime. This follows the visual approach of Matt
Pocock's `improve-codebase-architecture`, with rendering moved to generation
so the document also works offline.

Before the first Mermaid rendering, check for a working Mermaid CLI or an
available equivalent renderer that exports self-contained SVG. If neither is
available, stop before rendering and give this exact installation command:

```bash
npm install -g @mermaid-js/mermaid-cli
```

Keep independent document preparation runnable. With the CLI, use temporary
source and output files:

```bash
mmdc -i diagram.mmd -o diagram.svg
```

Follow the [Mermaid CLI documentation](https://github.com/mermaid-js/mermaid-cli)
for the installed renderer. Verify rendering succeeds before embedding. When
combining multiple SVGs, give their internal IDs and references unique prefixes
to prevent collisions. Keep needed SVG styles and remove external resource
dependencies; source attribution links may remain optional links.

## Verify and deliver

Read the saved HTML back and confirm every template marker is replaced. Inspect
resource attributes, CSS URLs/imports, module imports, and JavaScript requests
for dependencies outside the file. Check embedded SVG resources too.

When browser tooling is available, open the local file with networking blocked.
Inspect desktop and narrow layouts, diagrams, and any interactions; confirm the
page initiates no network requests. Fix clipping, missing content, or errors.
If browser verification is unavailable, perform the static checks and explicitly
report that offline rendering and layout were not visually verified.

Deliver a clickable link to the single HTML file and any material verification
limits. Temporary Mermaid sources and SVGs are not delivery dependencies.

## Attribution

Visual guidance comes from [Dex Horthy's `show-me`](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md),
used as a conditional runtime dependency. The architectural HTML approach is
also inspired by [Matt Pocock's `improve-codebase-architecture`](https://github.com/mattpocock/skills/tree/main/skills/engineering/improve-codebase-architecture).
