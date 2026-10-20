---
name: explain-changes
description: Explain what changed, why it matters, and the before-and-after behavior of local edits, a PR, commits, branches, releases, or a supplied diff. Use when the user wants to understand changes rather than assess whether to accept them.
---

# Explain Changes

Build a grounded explanation of a change: the problem or previous state, the
new behavior, and its practical effect on users, callers, or maintainers.
Deliver it in the conversation, respecting the user's language and technical
background. Scale the explanation to the substance of the change.

## Establish the comparison

Use the target already identified in the conversation. When none is identified,
inspect the working tree first. If it contains changes, explain those; otherwise
ask which PR, commit, branch, version, or patch the user means. Ask when multiple
plausible targets would lead to materially different explanations.

State the scope and baseline briefly so the reader knows what is being compared:

- **Local edits:** compare the working state with `HEAD`, covering staged and
  unstaged changes and relevant untracked files. Inspect both staged and unstaged
  diffs to catch intermediate changes that cancel out in the combined comparison.
  Respect an explicit request for only staged or only unstaged changes. Untracked
  files require reading their contents; they are absent from ordinary Git diffs.
- **PR:** retrieve its description, base and head revisions, diff, and relevant
  linked issues or specs using available repository-host tools. Use the PR's
  merge-base comparison and distinguish it from local edits. For a merged PR,
  use its recorded change rather than unrelated changes added to the base later.
- **Commit or commit range:** explain a single commit against its parent, or the
  net change between the requested endpoints. For a merge commit, identify the
  parent used; ask if the intended comparison is unclear.
- **Branch:** compare its tip with the merge base of the intended target branch
  to isolate the branch's own work. Infer the target from repository or PR
  metadata when available; state that choice.
- **Tags, versions, or two explicit revisions:** compare the two snapshots
  directly. Make the direction clear: older state to newer state, or the order
  explicitly requested by the user.
- **Supplied patch or diff:** use its declared comparison and available context.
  Explain what can be established from it and identify missing surrounding code
  or baseline evidence that limits the explanation.

Resolve Git references to revision IDs where available. Preserve the working
tree while inspecting historical states. If a remote target cannot be accessed,
report the missing access or tool and request the diff or an accessible target;
continue any explanation supported by the evidence already available.

## Reconstruct the change

Read the diff and enough surrounding code, callers, tests, and relevant
documentation to establish both states. Use issue or PR text for stated intent
and implementation evidence for actual behavior. Label inferred motivation and
explain discrepancies between the stated goal and the delivered change.

Group changes by behavior or purpose, following the affected flow across files.
Account for every substantive part of the selected change; summarize generated
files, lockfiles, and mechanical edits together unless they have a distinct
effect. Distinguish runtime changes from tests, documentation, configuration,
and internal restructuring. A refactor may preserve observable behavior while
changing responsibilities or future maintenance costs.

For each meaningful group, establish:

- What happened before, and what happens with the change.
- Who or what is affected, and in which concrete scenario.
- How the implementation produces that difference, with useful code references.
- Any migration, compatibility requirement, or tradeoff supported by the evidence.

Use the same representative input or caller scenario on both sides when an
example makes the difference clearer. Include setup, result handling, or failure
behavior when those change. When structure materially changes, a small Mermaid
or text diagram can show the affected responsibilities and dependency directions.
Ground examples and diagrams in the inspected revisions.

## Explain the result

Lead with the main change and its practical effect. Add only the background
needed to understand it, then walk through the meaningful groups in a logical
order. Support material claims with file paths, symbols, or source links that
belong to the state being described. Avoid substituting a file inventory or a
commit-message list for an explanation.

Distinguish observed behavior, intended behavior, and uncertainty. Describe tests
as evidence of covered scenarios; claim passing results only when supported by
an observed run, identifying its scope. Running checks is optional when it
resolves a question needed for the explanation.

Finish when the selected scope's substantive changes and their practical effects
are explained and evidence gaps are explicit. Keep this an explanation task:
editing code, creating commits, publishing comments, and conducting an acceptance
review require a separate request.
