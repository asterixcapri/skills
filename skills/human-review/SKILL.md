---
name: human-review
description: Guide a human through a PR that has completed implementation and AI code review, using code evidence to judge system properties, architectural tradeoffs, and residual risks before acceptance.
---

# Human Review

Help the human decide whether to accept a PR. This is the human judgment stage
following grill-with-docs, to-spec, to-tickets, implement-spec or implement,
and AI code review. Use the artifacts from that workflow as inputs; running
those upstream skills is outside this review.

The unit of review is a system property, traced through whichever files provide
evidence. Focus human attention on consequential decisions and uncertainty;
reserve line-by-line inspection for paths where it helps establish a property.

## Prepare the PR review

Use the PR already identified in the conversation. Ask for a missing target only
when it blocks useful preparation. Read repository guidance, the PR description,
base-to-head diff, linked spec and tickets, and available AI review results and
review discussions. Record the base and head revisions and distinguish local
changes from the PR being reviewed.

Summarize the intended behavior, the implemented approach, departures from the
spec, and the disposition of relevant AI findings. Treat the previous review as
an input to verify, not proof that a property holds. If an artifact is unavailable
or a prior step is incomplete, record the gap and continue independent preparation;
keep conclusions that depend on missing evidence explicitly unresolved.

Before assessing module depth, boundaries, interfaces, responsibilities, or test
seams, check that codebase-design is available and read it. If unavailable, stop
before that analysis and provide the exact installation command:

```bash
npx skills@latest add mattpocock/skills --skill codebase-design
```

Earlier preparation and independent behavioral questions remain runnable.

## Propose an agenda

Identify a short set of concrete properties affected by the PR, ordered by risk
and impact. Explain why each deserves human attention. Consider:

- **Problem fit:** implemented behavior serves the problem and acceptance criteria.
- **Structure and change cost:** module depth, responsibilities, architectural
  boundaries, interfaces, and seams hide complexity and localize likely changes.
  Assess abstractions by the complexity they save, rather than their number.
- **Behavior:** contracts, invariants, observable outcomes, error handling, and
  partial failures preserve the promises that matter.
- **High-risk paths:** relevant authorization, sensitive data, concurrency,
  migrations, and irreversible operations have sufficient supporting evidence.

Express items as claims to investigate, such as “the domain remains independent
of persistence” or “retrying this operation cannot duplicate a charge.” Select
properties relevant to this PR; these categories are lenses, not a mandatory
checklist. Include important risks the human has not already raised.

Present the orientation and proposed agenda, then let the human choose or adjust
the starting point. The human can reprioritize, add, deepen, or skip items.

## Discuss one property at a time

Trace the implementation, callers, dependencies, contracts, and relevant tests
far enough to assess the claim. Seek counterexamples as well as supporting
evidence: a dependency crossing the boundary, a path violating the invariant,
or a failure leaking through the interface. Ground architectural explanations
in actual code and distinguish test inspection from tests you executed.

Present a concise conclusion with precise code references, meaningful
counterexamples or the limits of the search, tradeoffs, and residual uncertainty.
Recommend whether to accept the current design or request an intervention, and
explain why. Keeping the code is a valid outcome. Separate fixes needed for this
PR from improvements suitable for later work.

Invite the human's judgment on this property and wait before advancing. Follow
requests for deeper evidence or alternative interpretations; challenge a
hypothesis when the code contradicts it. The human need not personally inspect
every supporting line, but must be able to assess the reasoning and its limits.
An agent recommendation becomes a decision only when the human accepts it.
Skipped or undecided properties remain open.

## Keep a durable review record

Maintain a short Markdown document using the repository's review-document
convention, or `human-review/pr-<number>.md` when none exists. Reuse an existing
record for this PR. Record:

- PR identity, reviewed base and head revisions, input artifacts, and evidence gaps.
- Each examined property, supporting and contrary evidence, the agent's
  recommendation, the human's decision and rationale, and residual uncertainty.
- Required interventions, deferred improvements, and open or skipped properties.
- Overall human judgment on the PR, or an explicit indication that it is pending.

Update the record as decisions change and link it for the human. Keep it concise;
reference evidence instead of copying the conversation. Writing this local review
record is part of the workflow. Publishing comments or a formal PR review requires
an explicit request, honoring authorization already given in the conversation.

## Close, pause, or request corrections

Stay in review mode until the human chooses to close, pause, or implement.
Acknowledging an individual point does not close the PR review. When the agenda
is exhausted, propose an overall judgment and ask the human to decide whether
the PR is acceptable, needs changes, or remains undecided.

Closure may leave open properties, but the record must state them and the limits
of the reviewed scope. A paused review remains unfinished and records the next
property to investigate. Neither closure nor acceptance automatically publishes
a review or merges the PR.

Implement corrections only on explicit request, preserving existing authorization
and following the repository's implementation workflow. After the PR changes,
refresh the diff and evidence against its new revision. Reopen affected properties
and identify new ones before relying on the earlier judgment; retain unaffected
decisions with their provenance.
