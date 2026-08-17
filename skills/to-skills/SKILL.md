---
name: to-skills
description: Turn behaviour observed during a session into changes to the skills that steer it, distinguishing portable procedure from project-specific rules, and invocation defects from execution defects. Use when a skill fired at the wrong time or failed to fire, when following a skill still produced the wrong work, when a procedure repeated across projects looks worth capturing as a skill, or when the user asks to update skills from recent work.
---

# To Skills

Turn observed agent behaviour into durable changes to the skills that steer it.

## Routing

Decide who owns the lesson before evaluating it:

- **Portable procedure** — true in another repository, language, or stack. It belongs to a skill; continue here.
- **Project rule** — true because of this repository's conventions, stack, or tooling. It belongs to project documentation. Hand it to /to-docs and stop carrying it here.

A lesson that presupposes this project's names, tracker, framework, or directory layout is a project rule, however procedural it sounds.

## Phase 1: Propose

Do not edit any skill during this phase.

1. Read the skills in play: those the repository publishes, those installed as dependencies, and the descriptions of both. Record which are **owned** — editable in place — and which are **installed**, tracked by a lock file and overwritten by the next install.
2. Reconstruct the evidence from the session. A skill change earns its place through observed failure: the agent ran under a given skill and the work still went wrong, or the skill never entered the run at all. State what the agent did, which skill was in effect, and where the outcome diverged from the intent. A lesson with no such run behind it is a `Defer`.
3. Locate the defect surface. A skill fails in one of three places, and the fix lives at the one that failed:
   - **Invocation** — the description. The skill fired on work it does not handle, or stayed silent on work it does. The body may be flawless.
   - **Execution** — the steps or reference. The agent read the skill, followed it, and still chose wrong: a missing step, a fuzzy completion criterion, a rule that reads as optional, guidance contradicted by another skill.
   - **Absence** — no skill owns the procedure, and the agent reinvented it.
4. Test the portability claim against a repository this one has never seen. Reject or generalize guidance that becomes wrong, meaningless, or merely inapplicable outside the project it came from.
5. Classify each candidate as one of:
   - **Edit an owned skill**: change the invocation or execution surface of a skill this repository publishes.
   - **Propose upstream**: the defect is in an installed skill. The outcome is a patch or report for its source, or an owned skill that covers the gap — never an in-place edit.
   - **New skill**: an absent procedure with a distinct trigger of its own.
   - **Already covered**: an existing skill states it adequately; the run failed for another reason.
   - **Keep local**: a project rule, routed to /to-docs.
   - **Defer**: a plausible pattern with a single accidental run behind it.
6. Prefer sharpening or replacing text over appending it. Every line added to a skill is paid on every invocation of that skill, in every project that installs it, whether or not the line fires. State what each proposed addition retires.
7. Present each proposal as: the skill and the surface it touches, the current text, the proposed text, the run that motivates it, the portability argument, and what it retires or contradicts elsewhere.
8. List the lessons rejected from the skills and explain why.
9. End by requesting approval. Do not edit any skill until the user explicitly approves it.

When the target is this skill, or a skill currently running, stop at the proposal and let the user apply it in a later session.

## Phase 2: Apply after approval

After explicit approval:

1. Use /writing-for-agents and apply its writing discipline throughout this phase. If it is unavailable, stop and ask the user to install it with `npx skills@latest add mattpocock/skills --skill writing-for-agents`.
2. Re-read every affected skill before editing it, including the descriptions of neighbouring skills whose triggers the change touches.
3. Edit owned skills only. For an installed skill, write the upstream patch or report instead, and say plainly that the local copy stays untouched because the next install replaces it.
4. Keep each changed description a working context pointer: the surviving triggers must still name distinct branches the body handles.
5. Follow the repository's own instructions for publishing skills — directory layout, naming, language, and the documentation it requires when a skill is added, renamed, or removed.
6. Run the repository's completion checks for a skill change, and report any check not run.
7. Summarize the skills changed, the surface changed on each, the upstream proposals produced, and the approved lessons routed to project documentation instead.

## Quality bar

A skill change earns its load when it would alter what the agent does on the run that failed, and would do so in a repository this one has never seen. Reject changes that restate what the agent already does by default, encode one project's conventions as universal procedure, or add a rule where a sharper trigger or a clearer completion criterion would fix the run.
