---
name: architecture-alternatives
description: Grill the user on a software problem and its constraints, then present three structurally different architectures using codebase-design, with comparable client code and a recommendation. Use when the user wants an interview before comparing architectural alternatives for a project, feature, or module.
---

# Architecture Alternatives

Establish a shared problem brief through `grilling`, then propose exactly three
architectures evaluated with `codebase-design`. Make the caller's experience
visible in actual code examples. Finish with a comparison and recommendation;
the user owns the selection.

## Ground the problem

Read the request, prior decisions, and project guidance. For an existing
codebase, inspect relevant entry points, callers, domain rules, and tests; cite
the files or symbols that establish decisive constraints. For a new project,
start from a concrete use case and distinguish known requirements from assumptions.
Discovery can begin before the dependencies below are available.

## Grill the requirements

Before the first interview round, check that Matt Pocock's `grilling` skill is
available and read its `SKILL.md`. If unavailable, stop before the interview and
give this exact installation command:

```bash
npx skills@latest add mattpocock/skills --skill grilling
```

Follow `grilling` for the design tree, frontier rounds, question format,
recommendations, fact-finding delegation, and waiting for answers. Keep stable
question identifiers. Scope the interview to the problem and its evaluation
criteria: intended behavior, caller needs, invariants, failure behavior, hard
constraints, and what makes a solution successful.

Leave module responsibilities, interface shape, and seam placement open for the
three proposals, except where an existing constraint genuinely fixes them.
Investigate environment facts rather than asking the user to supply them.

The interview is an explicit phase even when the initial request is detailed.
Use established answers instead of repeating questions; when the scoped frontier
is already empty, move directly to the shared brief and its confirmation.

## Confirm the shared brief

Close the interview only when the brief establishes:

- the problem, caller, desired outcome, and scope;
- one representative scenario with concrete inputs and observable results,
  including a meaningful failure;
- invariants and hard constraints, including behavior to preserve;
- success criteria for comparing the alternatives and architectural decisions
  deliberately left open.

Summarize these points and follow `grilling`'s shared-understanding confirmation
before producing alternatives, honoring confirmation already given in the
session. An unanswered recommendation is still open. A missing requirement that
could invalidate a proposal keeps the interview open; architecture choices
reserved for the proposals do not.

## Design three alternatives

Before the first assessment of module responsibilities, interfaces, or seams,
check that Matt Pocock's `codebase-design` skill is available and read its
`SKILL.md`. If unavailable, stop before that assessment and give:

```bash
npx skills@latest add mattpocock/skills --skill codebase-design
```

Earlier discovery and requirements grilling remain runnable. Resolve both
dependencies through the environment's skill catalog or installed locations,
rather than project-specific absolute paths. Use `codebase-design` as the source
of truth for vocabulary and design principles. Read its supporting references
when the design question calls for them.

Develop exactly three viable architectures against the confirmed brief. Keep
requirements and the representative scenario fixed. Differentiate the designs
through consequential choices about module responsibilities, interface shape,
and seam placement. Explain how each choice changes what the caller must know
or do. Technology labels, directory layouts, or implementation effort alone do
not establish a distinct alternative.

For each proposal, provide a compact design sheet:

1. **Design:** central choice, module responsibilities, and a sketch of the
   interaction showing where the seams live.
2. **Interface:** concrete contracts, including caller-visible invariants,
   ordering, configuration, and errors where relevant.
3. **Client code:** the walkthrough specified below.
4. **Depth and locality:** complexity hidden from callers, the deletion-test
   result, and which module and client code change under one plausible evolution
   grounded in the brief. Justify seams through demonstrated variation and
   identify their adapters.
5. **Verification and costs:** an illustrative behavior test through the same
   interface used by the caller, plus the principal tradeoffs and adoption or
   migration cost relevant to the project.

Replace cosmetic variants and designs that violate hard constraints before
presenting the set. If three viable designs require relaxing a confirmed
constraint, return to the user with that concrete conflict rather than silently
changing the brief.

## Show the client code

Include a code block for each proposal using the project's language, or clearly
labeled pseudocode if no language is established. Show the same representative
scenario and failure across all three:

- construction and composition, including supplied dependencies and configuration;
- realistic calls with concrete inputs and any required sequencing;
- consumption of the returned result and handling of the meaningful failure.

Expose setup and orchestration the caller must own; the client example must be
more than an isolated method call or an interface declaration. Identify the
observable result and effects. Keep client code, contracts, interaction sketches,
and behavior tests consistent. Mark the examples as illustrative unless they
have actually been implemented and executed.

## Compare and recommend

Present a compact comparison table covering depth, locality, seam placement,
testability, adoption cost, and the brief's success criteria. Support judgments
with concrete differences visible in the interfaces and client code.

Recommend one proposal, naming its decisive advantage, accepted tradeoff, and
the changed requirement that would favor another. Deliver the confirmed brief,
three design sheets with client code, and comparison as one coherent result.
This workflow ends with the recommendation. Further grilling to select a design,
specification writing, and implementation follow only when the user requests them.
