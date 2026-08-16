# Skills for Software Engineers

Reusable agent skills distilled from production software-engineering work. The
skills are small, focused, and designed to work across different projects and
coding agents.

## Available skills

### `execute-ticket-graph`

Turn the dependency graph written by `to-tickets` into an implementation run. The
skill reads every ticket and its blockers, calculates which tickets are ready, and
launches one fresh subagent for each ready ticket. Every subagent invokes the
project's existing `implement` workflow for exactly one ticket.

Independent tickets run concurrently in isolated Git worktrees, up to the
parallelism limit defined by the user, repository, and host agent. Tickets with
unresolved blockers wait. When a subagent finishes, the skill collects its verified
commit, integrates successful commits serially, verifies their combined state, and
only then releases the next group of tickets.

For example, given this graph:

```text
A ──┬──> B ──┐
    └──> C ──┴──> D
```

the skill runs the tickets in three rounds:

1. Run `implement` for A.
2. After A integrates and passes verification, run `implement` for B and C in
   parallel subagents.
3. After both B and C integrate and pass verification, run `implement` for D.

`implement` remains the sole owner of each ticket's lifecycle, including claims,
acceptance criteria, failures, and completion. The graph skill introduces no status
convention of its own. After each round, it rereads the ticket system and recomputes
the executable frontier instead of assuming that the graph is unchanged. Merge or
verification failures keep downstream work blocked.

Use `execute-ticket-graph` when an implementation effort contains multiple tickets
with blocking relationships and the project already defines its ticket-tracker and
`implement` workflows.

[View the skill](skills/execute-ticket-graph/SKILL.md)

#### Installation

```bash
npx skills@latest add asterixcapri/skills --skill execute-ticket-graph
```

#### Dependencies

Matt Pocock skills:

- `to-tickets` produces the dependency graph consumed by this skill.
- `implement` executes each ticket selected from the graph.

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
