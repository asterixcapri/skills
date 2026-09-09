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

### `human-review`

Guide human judgment on a PR after `grill-with-docs`, `to-spec`, `to-tickets`,
`implement-spec` or `implement`, and an AI code review. The agent uses the spec,
tickets, diff, and prior review to propose an agenda ordered by risk and impact.

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

## Discover skills

List the skills published by this repository without installing them:

```bash
npx skills@latest add asterixcapri/skills --list
```
