# Skills for Software Engineers

Reusable agent skills distilled from production software-engineering work. The
skills are small, focused, and designed to work across different projects and
coding agents.

## Available skills

### `execute-ticket-graph`

Execute implementation tickets as a validated dependency graph. The skill claims
the executable frontier, delegates independent tickets concurrently in isolated Git
worktrees, integrates successful commits serially, verifies the combined result,
and then releases downstream tickets.

Use `execute-ticket-graph` when an implementation effort contains multiple tickets
with blocking relationships and the project already defines its ticket-tracker and
single-ticket implementation workflows.

[View the skill](skills/execute-ticket-graph/SKILL.md)

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

## Installation

### `execute-ticket-graph`

```bash
npx skills@latest add asterixcapri/skills --skill execute-ticket-graph
```

### `to-docs`

During its apply phase, `to-docs` invokes Matt Pocock's `writing-for-agents`
skill to write the approved documentation changes. Install that dependency
first:

```bash
npx skills@latest add mattpocock/skills --skill writing-for-agents
```

Then install `to-docs`:

```bash
npx skills@latest add asterixcapri/skills --skill to-docs
```

List the skills published by this repository without installing them:

```bash
npx skills@latest add asterixcapri/skills --list
```
