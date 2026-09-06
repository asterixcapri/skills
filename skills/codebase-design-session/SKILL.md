---
name: codebase-design-session
description: Collaboratively design a software feature or architecture through concrete use cases, API contracts, data flows, and module responsibilities. Use when the user wants to work out how code should fit together or explore a redesign through proposals and walkthroughs.
---

# Codebase Design Session

Build a concrete design with the user. Bring proposals they can react to, trace
their consequences, and revise the design together. The agent owns investigation
and technical recommendations; the user supplies product intent and constraints
that cannot be established from the available evidence.

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

Propose the smallest coherent design that supports the use case, following the
codebase's existing conventions. Start from behavior and derive the routes and
code structure. Make the proposal inspectable with the artifacts that help:

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

## Walk through and revise

Trace one concrete interaction through the proposal. Follow the values as they
are validated, transformed, passed between modules, and returned. Check that every
business rule and state change has a clear owner. Then walk through the failure
or edge cases that could invalidate the design. Discuss authorization,
transactions, retries, concurrency, or compatibility where the use case makes
them relevant.

Recommend routine technical choices with their rationale. Bring alternatives
when they have materially different consequences, and explain the tradeoff in
terms of the use case. Reversible details can remain explicit working assumptions.
Separate observed facts, proposals, user-agreed decisions, and deferred questions.

Ask only the few questions needed to unblock the next useful revision, preferably
one at a time. Attach questions to the concrete proposal and include a recommended
answer when justified. Resolve unknown product rules before treating dependent
behavior as agreed; continue independent design work while they remain open.

Revise the affected contract, responsibilities, and flow together as answers
arrive. Keep a compact decision record with rationale and significant rejected
alternatives. Show changes that help the user evaluate the design. Use `grilling`
only when the user explicitly requests its intensive questioning workflow.

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

The design is ready for synthesis when the selected use case has a coherent
contract and flow, responsibilities are assigned, consequential failure cases
are addressed, and remaining uncertainties are explicitly resolved, deferred, or
identified as blockers. Completeness is relative to the selected scope; incidental
implementation details need not all be decided.

Provide a concise synthesis of the proposed design, agreed decisions and their
rationale, assumptions, and remaining questions. The user can continue, narrow
the scope, pause, or move forward; answering one question is not agreement with
the whole design. On pause, leave enough context to resume at the open point.

The planning path is `codebase-design-session` → `to-spec` → `to-tickets`.
When the user requests that path, pass the design synthesis to `to-spec`, then
pass the resulting specification to `to-tickets`. A request for the complete
sequence authorizes continuing through both steps without a new confirmation
between them, subject to their applicable instructions and project configuration.
Check each dependency when its step begins.

Carry forward the problem, scope, contracts, responsibilities, interactions,
agreed decisions and rationale, significant rejected alternatives, behavior to
preserve, and verification approach. Preserve assumptions and deferred questions
as such. Treat settled decisions as inputs; reopen them only if new evidence or
a contradiction requires it. Let the receiving skill own its artifact format.
Resolve blockers for each action before proceeding. If a blocker remains, keep
the synthesis available and identify exactly what prevents the handoff.

If the user requests implementation directly, carry the same context into the
repository's implementation workflow. The session does not impose planning steps
the user has chosen to skip.
