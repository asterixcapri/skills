---
name: ablate
description: Reduce working code to its essential complexity by removing one layer at a time and checking that behavior is preserved after each removal, keeping only what breaks the build, types, or tests when taken out. Use when the user wants to strip over-engineering, dead abstractions, and speculative layers from code that already works.
---

# Ablate

Cut working code to the bone: remove the useless layers one at a time, and keep
only what earns its place by breaking something real when it comes out.

Ablation is subtraction under a safety net, not redesign. Observable behavior
stays identical from the first removal to the last. If removing something would
change what the code does, it was load-bearing — leave it.

## Establish a green signal first

Run the signal that will prove each removal preserves behavior — the test
suite, the build, the type checker, a linter, or a reproducible run of the
affected path — and confirm it is green before you remove anything.

No signal for the code in scope is the first finding. Either add a
characterization test pinning the current behavior of what you will touch, or
stop and tell the user a signal is needed. Keep any independent exploration
runnable meanwhile.

## Inventory the candidate layers

The green signal decides what is behavior-preserving to remove, not which seams
earn their keep for design reasons. Before judging candidates by structure,
check that Matt Pocock's `codebase-design` skill is available and read its
`SKILL.md`. If unavailable, stop before that judgment and give this exact
installation command:

```bash
npx skills@latest add mattpocock/skills --skill codebase-design
```

The green-signal baseline and any removal the signal alone settles stay runnable
without it. Use `codebase-design` as the model of essential structure: its
vocabulary and deletion test separate accidental complexity to ablate from a
load-bearing seam to keep. A shallow pass-through that adds no leverage is a
candidate; a deep module — much behavior behind a small interface — or a seam
something real varies across stays, even where removing it would keep the signal
green.

List the removable candidates, ordered safest-and-outermost to
riskiest-and-most-load-bearing. Look for:

- **Pass-through indirection:** wrappers, one-line delegating methods, facades
  over a single implementation, adapters that only rename.
- **Single-use abstractions:** an interface with one implementation, a factory
  that builds one thing, a strategy with one strategy, a generic only ever
  instantiated with one type.
- **Speculative extensibility (YAGNI):** config flags, hooks, and parameters
  that nothing varies, options always passed the same value, "just in case"
  seams with no second caller.
- **Dead and unreachable code:** unused exports and helpers, branches that
  cannot be taken, permanently-on or permanently-off feature flags.
- **Premature structure:** managers, services, and coordinators that only
  shuffle data between other layers; inheritance used only to share code; an
  event bus for a single synchronous caller.
- **Redundant guarding:** caches nothing reads twice, re-validation of
  already-validated data, defensive checks for states that cannot occur.

## Ablate one candidate at a time

Work the list in order. For each candidate:

1. Remove it — delete it, or inline it into its single call site.
2. Re-run the signal.
3. Green keeps the removal. Red reverts it immediately; record what the signal
   was protecting — that is why the layer stays.

One concern per step keeps each change reviewable and makes a later
"load-bearing" a clean single revert. When a thing has exactly one use, inline
it into the caller rather than leave a hole.

## Re-measure and repeat

Each kept removal shrinks the surface for the next, so re-scan: collapsing one
wrapper often leaves its now-single caller inlineable in turn. Continue until no
candidate comes out without turning the signal red. That red boundary is the
essential complexity — stop there.

## What not to ablate

Keep these and treat them as load-bearing:

- Anything whose removal changes observable behavior. A test whose expectation
  must change marks live code — stop and reconsider.
- A deep module or a seam with real second use, even when removing it would
  keep the signal green. Apply `codebase-design`'s deletion test: if the same
  complexity reappears across callers once it is gone, it was earning its keep,
  not passing through. Two or more realized adapters mark a real seam; one is
  only hypothetical. Keep a public API or extension point consumed from outside
  the repo. One in-repo caller is not proof there is only one — check before
  collapsing a boundary others may depend on.
- Error handling, trust-boundary validation, and security or permission checks.
  A passing happy path does not prove these are unused; the signal may simply
  not cover the failure case. Keep them unless you can show the case is
  impossible.
- A layer with a stated, documented reason (performance isolation, an
  architectural boundary). Flag it to the user rather than remove it yourself.

## Report what came out

Summarize: what was ablated and why it was safe, what stayed and the concrete
thing that broke when you tried to remove it, and the net reduction in lines,
files, and dependencies. Behavior matches where you started — say so, and point
at the still-green signal as the proof.
