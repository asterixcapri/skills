# Skills for Software Engineers

Reusable agent skills distilled from production software-engineering work. The
skills are small, focused, and designed to work across different projects and
coding agents. They build on and complement
[Matt Pocock's skills](https://github.com/mattpocock/skills), extending that work
with workflows designed to be used alongside them.

## Available skills

### `architecture-alternatives`

Start with an explicit `grilling` phase to establish the problem, intended
behavior, constraints, and success criteria. After confirming the shared brief,
produce exactly three architectures using `codebase-design`, leaving module
responsibilities, interface shape, and seam placement open for the alternatives.

Each proposal includes concrete client code for the same scenario: dependency
setup, calls, result handling, and a meaningful failure. Interface contracts,
behavior-test examples, and a comparison of depth, locality, testability, and
adoption cost make the tradeoffs inspectable. The workflow ends with a
recommendation; selection remains with the user.

Use this skill to clarify requirements through an interview before comparing
three concrete architectural alternatives.

[View the skill](skills/architecture-alternatives/SKILL.md)

#### Installation

```bash
npx skills@latest add asterixcapri/skills --skill architecture-alternatives
```

#### Dependencies

Discovery can begin immediately. Matt Pocock's `grilling` is required before the
interview; `codebase-design` is required before module and interface assessment:

```bash
npx skills@latest add mattpocock/skills --skill grilling
npx skills@latest add mattpocock/skills --skill codebase-design
```

---

### `explain-changes`

Explain what changed, why it matters, and how behavior differs before and after.
Accept local edits (staged, unstaged, and new files), a PR, commits, a branch,
two tags or versions, or a supplied patch. Establish the comparison first, then
trace the diff through relevant code, callers, tests, and documentation.

Group the explanation by behavior or purpose, with concrete before-and-after
examples and code references. Distinguish stated intent from implemented
behavior and make evidence gaps explicit. Deliver the explanation in the
conversation, in the user's language, with detail proportional to the change.

[View the skill](skills/explain-changes/SKILL.md)

#### Installation

```bash
npx skills@latest add asterixcapri/skills --skill explain-changes
```

---

### `human-review`

Guide human judgment on a PR after `grill-with-docs`, `to-spec`, `to-tickets`,
`implement-spec` or `implement`, and an AI code review. The agent first explains
the original problem, the improvement the PR pursues, and the caller-facing API
before and after the change. Material architectural changes are shown with
evidence-based before-and-after Mermaid or text diagrams.

The agent then uses the spec, tickets, diff, and prior review to propose an agenda
ordered by risk and impact.

Review system properties: problem fit, module depth, seams, interfaces,
architectural boundaries, invariants, observable behavior, and high-risk paths.
The agent presents code evidence, seeks counterexamples, and discusses one point
at a time. You choose priorities and judge whether the tradeoffs are acceptable.

A short Markdown review record preserves evidence, human decisions, requested
interventions, and open questions. You decide when to close or pause; corrections
and publishing a PR review require an explicit request. Changes to the PR reopen
the affected properties for review.

[View the skill](skills/human-review/SKILL.md)

#### Installation

```bash
npx skills@latest add asterixcapri/skills --skill human-review
```

#### Dependencies

Matt Pocock's `codebase-design` is required before assessing module depth,
boundaries, interfaces, responsibilities, or test seams:

```bash
npx skills@latest add mattpocock/skills --skill codebase-design
```

PR preparation and independent behavioral questions can begin without it.
Upstream workflow artifacts are inputs; their producing skills are not runtime
dependencies of this review.

---

### `ablate`

Inspired by [Salvatore Sanfilippo (antirez)](https://antirez.com/).

Cut working code to its essential complexity. This skill establishes a green
signal — the test suite, build, or type checker — and proposes concrete
simplifications, recommending what to remove, keep, or defer. You approve the
proposal or select a subset before any edits, including prerequisite tests.

After approval, it removes one candidate layer at a time and checks that
behavior is preserved, reverting anything that breaks. New candidates beyond
the approved scope return to you as a follow-up proposal.

[View the skill](skills/ablate/SKILL.md)

#### Installation

```bash
npx skills@latest add asterixcapri/skills --skill ablate
```

#### Dependencies

Matt Pocock's `codebase-design` is required before judging which candidate
layers are accidental complexity to remove and which seams are load-bearing to
keep:

```bash
npx skills@latest add mattpocock/skills --skill codebase-design
```

Establishing the green signal and signal-driven investigation can begin without
it. Applying changes requires approval of the concrete proposal.

---

### `to-docs`

Turn decisions made during implementation, review, or design work into durable
project guidance. The skill separates project-wide rules from local fixes,
already-documented guidance, and patterns that are not mature enough to adopt.
It proposes documentation changes first and applies them only after explicit
approval.

Use `to-docs` when recent work has surfaced decisions that may deserve a place
in project documentation, especially:

- after implementing or reviewing a feature that introduced a reusable pattern;
- after a design discussion resolved conventions, boundaries, or exceptions;
- before adding lessons from a specific bug or route to `AGENTS.md`, coding
  standards, architectural guidance, or other project-wide documentation;
- when existing guidance may need refinement rather than another overlapping
  rule.

Applied repeatedly, `to-docs` improves the repository-level coding-agent
harness: the instructions and documentation that steer the agent's behavior. It
strengthens context engineering by turning decisions from real work into
consistent, discoverable guidance, keeping the agent aligned with project
conventions while preventing contradictory or overly specific rules from
accumulating.

The skill is most valuable when the difficult question is not how to write the
documentation, but whether a decision is durable and general enough to document
at all.

[View the skill](skills/to-docs/SKILL.md)

#### Installation

```bash
npx skills@latest add asterixcapri/skills --skill to-docs
```

#### Dependencies

Matt Pocock skill:

- `writing-for-agents` writes the approved documentation changes.

---

### `to-markdown`

Save the current discussion or a specified topic as a standalone Markdown
document. Preserve context, decisions, tradeoffs, and open questions. Use
`show-me` only when a visual explanation helps, with Mermaid, trees, pseudocode,
or diffs embedded as fenced blocks.

[View the skill](skills/to-markdown/SKILL.md)

#### Installation

```bash
npx skills@latest add asterixcapri/skills --skill to-markdown
```

#### Dependencies

[Dex Horthy's `show-me`](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md)
is required only before producing a visual explanation:

```bash
npx skills@latest add humanlayer/skills --skill show-me
```

Text-only documents can be produced without it. The default destination is
`docs/explanations/<topic>.md`, unless the user or project specifies another.

---

### `to-html`

Save the current discussion or a specified topic as one self-contained HTML
document that opens locally and works offline. A common template provides
responsive typography, tables, code, figures, and print styles; content determines
the sections. CSS, scripts, and images are embedded, with no CDN dependencies.

Use `show-me` only when a visual explanation helps. Mermaid diagrams are rendered
to SVG during generation and embedded in the page, with no Mermaid runtime in
the delivered file. The visual approach is also inspired by Matt Pocock's
`improve-codebase-architecture`.

[View the skill](skills/to-html/SKILL.md)

#### Installation

```bash
npx skills@latest add asterixcapri/skills --skill to-html
```

#### Dependencies

[Dex Horthy's `show-me`](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md)
is required only before producing a visual explanation:

```bash
npx skills@latest add humanlayer/skills --skill show-me
```

Mermaid diagrams additionally require a working SVG renderer, such as
[Mermaid CLI](https://github.com/mermaid-js/mermaid-cli):

```bash
npm install -g @mermaid-js/mermaid-cli
```

Text-only documents require neither dependency. The default destination is
`docs/explanations/<topic>.html`, unless the user or project specifies another.

---

### `crystal-clear`

Make responses and prose easier to understand and quicker to read. Lead with
the main point, use plain language, and remove repetition while preserving
the reasons, conditions, and details the reader needs to act.

Use this skill when asking for a clearer explanation, a shorter response, or
less verbose writing. It respects the requested language, audience, and format.

[View the skill](skills/crystal-clear/SKILL.md)

#### Installation

```bash
npx skills@latest add asterixcapri/skills --skill crystal-clear
```

---

## Discover skills

List the skills published by this repository without installing them:

```bash
npx skills@latest add asterixcapri/skills --list
```
