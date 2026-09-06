---
name: architecture-brainstorming
description: Analyze relevant code and propose concise architectural solutions in a collaborative brainstorming session before implementation. Use when the user wants to explore solutions, challenge design choices, and shape API contracts, data flows, and module responsibilities together.
---

# Architecture Brainstorming

Work with the user as senior developers discussing a project's architecture
before implementation. The central activity is collaborative brainstorming:
bring ideas, build on each other's reasoning, challenge weak choices, compare
alternatives, and evolve a shared design. Both participants can contribute
technical solutions and question the direction. Take responsibility for
investigation and recommendations, drawing on the user's knowledge of intent
and constraints that the available evidence cannot establish.

Lead each substantive exchange with a concrete recommendation grounded in the
current investigation. Focus on the few points that most affect the use case;
explain each in a short paragraph or compact point card, expanding only when the
user asks or the decision needs more detail. Follow promising threads and revisit
earlier proposals when insight changes them.
Weigh the engineering perspectives relevant to the decision, such as domain
correctness, maintainability, operation, and caller experience, in your own voice.
The senior-team framing describes the quality of reasoning and collaboration;
it does not require fictional participants or a scripted meeting.

Use the workflow below to support that conversation. Numbered open points are
its memory and transition check, not an agenda to exhaust on every turn. Keep
the register current, surface the points relevant to the current discussion,
and show the full open list at synthesis or a requested transition. Resolving
points should improve the shared design, not merely complete a questionnaire.

## Ground the session

Start with the use case and scope already given. Read repository guidance and the
relevant entry points, contracts, domain vocabulary, and tests. Distinguish current
behavior from proposed behavior. In a new project, work from the stated needs and
label assumptions about the environment.

Establish enough context to describe one representative interaction: the actor,
desired outcome, inputs, and constraints that could change the design. Ask for a
missing fact only when it prevents useful progress and cannot be investigated.
Keep discovery proportional to the selected use case.

## Load design guidance when needed

Before the first analysis of module responsibilities, interfaces, or test seams,
check that Matt Pocock's `codebase-design` is available and read it. Use its shared
vocabulary and principles to assess the proposed design; keep HTTP routes and
payloads concrete within that vocabulary. If unavailable, stop before that design
analysis and provide:

```bash
npx skills@latest add mattpocock/skills --skill codebase-design
```

Earlier discovery and use-case clarification remain runnable. Read supporting
references from that skill only when the design question calls for them.

## Put a proposal on the table

For each identified design point, inspect the relevant implementation and trace
its callers, dependencies, and tests far enough to explain the current behavior
and the consequence of changing it. Cite the decisive files or symbols. Resolve
code-answerable questions through this investigation before presenting the point;
when evidence is unavailable, state the gap and make the proposal provisional.
For a new project, ground it in a concrete scenario and explicit assumptions.

Propose the smallest coherent design that supports the use case, following the
codebase's existing conventions. Start from behavior and derive the routes and
code structure. Name the responsibility, interface, or dependency that changes,
where it belongs, and how the affected interaction would work afterward. Prefer
changes that address an observed consequence or stated requirement; retain the
current design when further abstraction has no demonstrated benefit. Make the
proposal inspectable with the artifacts that help:

- **Contract:** HTTP method and route, path/query/body attributes, which values
  come from the caller versus authentication or server state, required/optional
  fields, validation, response shape, and meaningful errors. Include an example
  request and response when designing an HTTP interaction.
- **Responsibilities:** the modules, classes, or functions involved, what each
  owns, its interface, and its dependencies. Explain where domain decisions and
  persistence live; introduce abstractions where they hide meaningful complexity.
- **Flow:** a short sequence diagram or code sketch showing how an input reaches
  its outcome, including reads, writes, and external effects.

Select the useful artifacts rather than filling every category for every task.
Use the equivalent entry point for jobs, events, libraries, or UI interactions.
Keep sketches provisional and identify assumptions that could change their shape.

## Critique, explore, and revise

Trace one concrete interaction through the proposal. Follow the values as they
are validated, transformed, passed between modules, and returned. Check that every
business rule and state change has a clear owner. Then walk through the failure
or edge cases that could invalidate the design. Discuss authorization,
transactions, retries, concurrency, or compatibility where the use case makes
them relevant.

Evaluate ideas on their merits regardless of who proposed them. When an idea is
unsuitable, say so plainly and explain the specific weakness: a broken invariant,
unclear ownership, excessive coupling, unnecessary complexity, or a mismatch with
the use case. Ground the criticism in code, a concrete scenario, or an explicit
constraint; distinguish demonstrated defects from risks and preferences.

Pair consequential criticism with a concrete repair or alternative. Lead with
the recommended solution, its expected benefit, and its main tradeoff. Compare
another option only when it represents a materially different choice. Seek
simplifications and stronger alternatives proactively, including flaws in your
own first proposal. Agreement should follow evaluation; disagreement should earn
its place through evidence rather than becoming a performance of skepticism.

Recommend routine technical choices with their rationale. During exploration,
reversible details can remain explicit working assumptions. Separate observed
facts, proposals, agreed decisions, and open questions. If the user knowingly
accepts a tradeoff, record the consequence and work within that decision; revisit it when
new evidence changes the assessment.

Ask only the few questions needed to unblock the next useful revision, preferably
one at a time. Attach questions to the concrete proposal and include a recommended
answer with its rationale. Resolve unknown product rules before treating dependent
behavior as agreed; continue independent design work while they remain open.

Revise the affected contract, responsibilities, and flow together as the discussion
or investigation reveals improvements. Show the improved proposal rather than
leaving the user with a list of objections. Keep a compact decision record with
rationale and significant rejected alternatives. Show changes that help the user
evaluate the design. Use `grilling` only when the user explicitly requests its
intensive questioning workflow.

## Bring in focused skills

Use Matt Pocock's other skills when a specific need arises:

| Need | Skill | Installation command if unavailable |
| --- | --- | --- |
| Resolve domain terminology or relationships; record a glossary or ADR | `domain-modeling` | `npx skills@latest add mattpocock/skills --skill domain-modeling` |
| Verify external technical facts against primary sources | `research` | `npx skills@latest add mattpocock/skills --skill research` |
| Test a design uncertainty with a throwaway state/logic or UI prototype | `prototype` | `npx skills@latest add mattpocock/skills --skill prototype` |
| Turn the selected design into a specification, when requested | `to-spec` | `npx skills@latest add mattpocock/skills --skill to-spec` |
| Turn the specification into tickets, when requested | `to-tickets` | `npx skills@latest add mattpocock/skills --skill to-tickets` |

Check availability and read the relevant skill at the first dependent step. If
missing, stop before that action, give its exact command, and keep independent
work runnable. These are conditional workflows, not prerequisites for the entire
session. Keep each invocation within the user's scope: a design discussion alone
does not authorize production implementation, publishing, or ticket creation.
Carry forward authorization already given for documentation, prototypes, or
implementation without asking for it again.

## Synthesize or transition

Keep a concise synthesis available during exploration, including the proposed
design, decisions and rationale, and an explicit numbered register of open points.
Assign each point a stable, increasing number when first raised. Preserve its
number across turns, summaries, and pauses; never renumber remaining points or
reuse a resolved point's number. Interpret references such as "point 3" against
this register. Retain resolved points with their resolution in the decision
record, separate from the open list. A reopened point keeps its original number.

Whenever presenting an unresolved point, including a single point or a recap,
show its number and your recommendation together. Use this compact format:

```text
Point 3 — <title>
Evidence: <observed behavior and consequence; decisive file/symbol or scenario>
Recommendation: <concrete architectural change, expected benefit, main tradeoff>
To resolve: <evidence to obtain or decision needed from the user>
```

When showing multiple points, present them in number order with these explicit
labels so the renderer preserves their identifiers even when numbers have gaps.
If evidence is missing, label the recommendation provisional and investigate what
would validate it. Resolve technical facts through investigation; obtain the
user's decision where product intent or a consequential tradeoff requires it.
A recommendation alone does not settle a question that needs the user's answer.

Before invoking `to-spec`, `to-tickets`, or starting implementation, check that
the selected use case has a coherent contract and flow, responsibilities are
assigned, consequential failure cases are addressed, and no numbered point
remains open. Every identified design question must have a recorded resolution, and
working assumptions must be validated or explicitly accepted as decisions.
Deferred questions and questions labelled non-blocking still prevent transition.
Do not use specification, ticket creation, or implementation to resolve them.

While points remain open, continue investigation, proposals, and discussion.
Only an explicit scope change by the user can move an unresolved point outside
the session's scope; record that exclusion and check that the retained design
does not depend on it. Incidental coding details need not be invented as design
questions. On pause, leave the unresolved points and proposed resolutions ready
to resume. Answering one question is not agreement with the whole design.

The planning path is `architecture-brainstorming` → `to-spec` → `to-tickets`.
When the user requests that path and the readiness check passes, pass the design
synthesis to `to-spec`, then pass the resulting specification to `to-tickets`.
A request for the complete
sequence authorizes continuing through both steps without a new confirmation
between them, subject to their applicable instructions and project configuration.
Check each dependency and repeat the readiness check when its step begins. If a
later step exposes a new open design point, pause that workflow and resolve the
point through the same proposal-and-discussion process before continuing.

Carry forward the problem, scope, contracts, responsibilities, interactions,
agreed decisions and rationale, significant rejected alternatives, behavior to
preserve, verification approach, and explicit scope exclusions. Treat settled
decisions as inputs; reopen them only if new evidence or
a contradiction requires it. Let the receiving skill own its artifact format.
If any open point remains, keep the synthesis available and identify exactly
what prevents the handoff, together with the recommended resolution.

If the user requests implementation directly, apply the same readiness check
before entering the repository's implementation workflow with the design context.
Skipping planning steps does not bypass resolution of open design points.
