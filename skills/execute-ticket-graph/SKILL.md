---
name: execute-ticket-graph
description: Read implementation-ticket dependency graphs produced by `to-tickets` and execute them in dependency order across any project-configured ticket system. Use when a multi-ticket effort has blocking edges and the host agent should claim each executable frontier, launch one fresh subagent per ticket to invoke `implement`, run independent tickets in parallel within the applicable parallelism directives, integrate successful commits serially, verify the combined state, and then release downstream tickets. Do not use for a single ticket or for Wayfinder decision maps.
---

# Execute Ticket Graph

Consume the ticket dependency graph written by `to-tickets`. Repeatedly select the
executable frontier, launch one fresh subagent per ticket, and require each subagent
to invoke the project's `implement` skill or equivalent. Run independent tickets in
parallel and hold dependent tickets until their blockers integrate and pass
verification.

## Preserve the seams

- Treat the configured ticket system as the source of truth for ticket identity,
  state, blocking edges, claims, comments, and completion.
- Read `docs/agents/issue-tracker.md` and repository instructions before querying or
  mutating tickets. If the tracker contract is incomplete, read
  [references/tracker-contract.md](references/tracker-contract.md) and settle only
  the missing operations before continuing.
- Never require local Markdown when the project uses GitHub, GitLab, Linear, Jira,
  or another system.
- Normalize tracker data before graph computation. Resolve resource paths relative to
  this `SKILL.md`, then run
  `node <skill-path>/scripts/inspect-graph.mjs <snapshot.json> --rounds` against the
  canonical shape in [references/tracker-contract.md](references/tracker-contract.md).
- Delegate exactly one ticket to each subagent. Require that subagent to invoke the
  existing `implement` skill or the project's documented equivalent.
- Do not use this workflow to execute a Wayfinder map. Wayfinder tickets resolve
  decisions; implementation tickets deliver code.

## Execute the graph

### 1. Establish policy

Resolve these values from repository instructions, tracker documentation, or the
user's invocation:

- effort reference and ticket scope;
- maximum concurrency from the applicable user, repository, and host-agent
  parallelism directives, defaulting to `2` when none is specified and never
  exceeding the available subagent slots;
- eligible, claimed, failed, and blocker-satisfied meanings;
- atomic claim operation and claim ownership evidence;
- integration target, worktree parent directory, and branch naming convention;
- dependency restoration needed in a fresh worktree;
- verification required from each worker before collection;
- fast verification after each integration and full verification after fan-in;
- integration mode, defaulting to supervised;
- retry limit, defaulting to `0`;
- whether a successful sibling may integrate when another worker fails, defaulting
  to yes after the supervised integration checkpoint.

Do not invent a state-changing tracker operation. Stop before fan-out if exclusive
claiming or blocker satisfaction cannot be determined.

### 2. Discover and normalize

Use the tracker contract to list every implementation ticket in the supplied effort.
Fetch enough data to determine identity, title, reference, execution state, and all
incoming blocking edges. Map the results to a canonical snapshot without changing
the tracker.

Validate the snapshot with `scripts/inspect-graph.mjs`. Do not launch workers if an
identity is duplicated, a blocker is missing, an edge is malformed, or the graph is
cyclic.

For a dry run, print:

- the current frontier and simulated rounds;
- ticket-to-worker assignments;
- proposed claim operations;
- base commit, branch names, and worktree paths;
- verification and integration commands.

Perform no writes during a dry run.

### 3. Claim one frontier

Select eligible frontier tickets up to the concurrency cap. Claim every selected
ticket before launching any worker, using the tracker-specific exclusive claim
operation. Record claim ownership evidence for the run.

After claiming, reread each selected ticket. Launch it only if the claim still belongs
to this run and its blockers remain satisfied. If acquisition is only process-local,
state that limitation; do not claim safety across machines.

### 4. Isolate writes

Resolve the exact base commit containing all previously integrated work. Give every
ticket a distinct branch and absolute worktree path from that same base. Use safe
Git commands that reject existing branches and worktree paths.

Restore dependencies inside each worktree as the repository requires. Copy no
secrets, ignored files, or dirty-checkout contents unless repository instructions
explicitly allow them.

### 5. Fan out to `implement`

Launch all selected subagents before waiting for any result. Start every subagent
with a fresh context and give it this packet:

```text
Ticket: <tracker reference and exact id>
Parent spec/effort: <reference>
Worktree: <absolute path>
Branch: <branch>
Base commit: <commit>
Required workflow: invoke the project's implement skill or documented equivalent
Verification: <project commands>
Completion report: outcome, commit, verification, dirty state, blockers
```

Tell each subagent to reconstruct context from repository instructions, the assigned
ticket and comments, its parent spec, domain and architecture docs, and relevant
code. Tell it not to select or update any other ticket.

### 6. Collect

Wait for every worker in the frontier. Preserve successful sibling results when one
worker fails. Accept a worker for integration only when it:

- implemented its assigned ticket and no other ticket;
- produced a commit on its assigned branch;
- passed the required worker verification;
- left no unexplained dirty state;
- reported the exact commit hash.

Record failed or escalated outcomes through the tracker contract. Never translate a
stopped worker into a resolved ticket.

### 7. Fan in serially

Keep the main checkout single-writer. At the supervised checkpoint, present the
eligible commits and failed siblings. Integrate successful commits one at a time in a
deterministic order, running the fast gate after each integration.

On conflict, stop automatic integration or invoke the project's explicit
merge-conflict workflow. Do not guess between competing intent.

After all selected commits integrate, run the full verification suite against their
combined state. Mark a ticket complete in the tracker only after its commit is in the
integration branch and all required gates pass. Leave failed and non-integrated
tickets visibly unresolved.

### 8. Recompute

Reread the tracker rather than mutating the old snapshot. Normalize and validate the
new graph, then compute the next frontier. Start another fan-out only after the
previous fan-in completes.

Stop when all scoped tickets are resolved or the graph has no executable frontier.
For a stalled graph, report each unresolved ticket, its state, and the blockers or
claims preventing execution.

## Safety invariants

- Persist claims before worker launch.
- Use one worktree and branch per concurrent writer.
- Use one writer for integration and tracker completion transitions.
- Never stage or commit unrelated changes from the user's checkout.
- Resolve exact branch and worktree paths before cleanup; never make cleanup part of
  a success assumption.
- Bound retries and expose exhausted work to humans.
- Treat combined fan-in verification as the correctness boundary.
