---
name: human-review
description: Accompany a human-led code review of a PR, diff, branch, module, or files. Use when the user wants to ask questions about existing code, explore simplifications or redesigns, and carry agreed decisions into to-spec and to-tickets. Automated review reports remain inputs to the discussion.
---

# Human Review

The user steers the investigation. Help them understand the code, challenge
assumptions, and decide what deserves to change. Keep exploration distinct from
implementation and preserve enough reasoning to continue into planning.

## Establish the scope

Use the target already given in the conversation. Ask for a missing target only
when it prevents useful investigation. Read applicable repository guidance and
give a brief orientation to the relevant responsibilities, entry points, and
dependencies; move directly to an existing question when orientation adds little.

For a PR, read its description, base-to-head diff, and available review discussions.
Establish which revision is being reviewed and distinguish it from local changes.
Use existing automated findings, including code-review output, as leads to verify.
If PR context is inaccessible, state the limitation and continue with available
code without implying the discussions were reviewed.

## Follow the questions

For each question, inspect the relevant implementation, callers, contracts, and
tests far enough to assess the proposed change. Answer with concrete code
references, distinguish evidence from inferred intent, and explain the consequences
of keeping, simplifying, removing, or redesigning the code as relevant. Recommend
an option and give its tradeoffs; challenge the user's hypothesis when the code
does not support it. Keeping the current code is a valid conclusion.

Use codebase-design when a question requires reasoning about module boundaries,
interfaces, responsibilities, or test seams. At the first such question, check
that the skill is available and read it. If unavailable, stop before that design
analysis and give the exact command:

```bash
npx skills@latest add mattpocock/skills --skill codebase-design
```

Earlier orientation and independent review questions remain runnable. Reach for
other skills only when a concrete question calls for their workflow; check their
availability at that point and, if missing, provide their exact installation
command before the dependent action.

Raise closely related concerns when they affect the answer, while leaving the
direction with the user. Keep the exchange conversational rather than turning it
into an exhaustive audit or mandatory questionnaire.

Maintain a compact record in the conversation of observations, hypotheses, agreed
decisions with rationale, rejected alternatives, and open or deferred questions.
Update it as conclusions change; show only useful updates during exploration.
An exploratory question such as “could we delete this?” remains a hypothesis until
the user agrees on an intervention. For PRs, distinguish changes needed to complete
the PR from improvements for later work.

Review mode authorizes investigation and proposals. Change code when the user
requests implementation, carrying forward any existing authorization; a speculative
question alone does not authorize edits. Publishing PR comments or a formal review
requires the user's request to do so.

## Let the user end or transition the review

Remain in review mode across turns until the user signals a transition. Answering
the latest question or receiving an acknowledgment does not end the review. You
may suggest a synthesis when the discussed points are resolved, but the user
decides whether to close, pause, or continue exploring.

- **Close:** summarize the reviewed scope, agreed decisions and rationale, open
  questions, and deferred work. Closure can leave uncertainties or no code changes.
- **Pause:** leave a concise resumption note with the current question, relevant
  code references, decisions so far, and the next investigation. Keep its status
  explicitly unfinished.
- **Move to planning:** prepare the selected intervention for to-spec with its
  problem, intended benefit, agreed solution, alternatives rejected, behavior to
  preserve, verification approach, and out-of-scope work. Separate independent
  interventions; use the user's selection or ask which to advance if ambiguous.
  Identify unresolved questions that block a buildable spec and resolve those
  before proceeding. Preserve other uncertainties explicitly.
- **Implement:** follow the user's requested implementation scope and the
  repository's implementation workflow, using the review's agreed decisions.

Closing the review does not automatically publish a spec or create tickets.
When the user requests to-spec or to-tickets, check that skill's availability at
the transition, then follow its instructions and project tracker configuration.
If unavailable, stop before that action and provide the corresponding command:

```bash
npx skills@latest add mattpocock/skills --skill to-spec
npx skills@latest add mattpocock/skills --skill to-tickets
```

Keep the review context available through the requested planning steps so they
can synthesize the discussion. Let those skills own spec and ticket formats;
the review's synthesis is their input. A request to proceed through both steps
is sufficient authorization to continue through that sequence under their
applicable instructions.
