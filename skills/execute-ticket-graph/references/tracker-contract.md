# Tracker contract

Use the project's configured ticket system through semantic operations. The backing
system may be local files, GitHub, GitLab, Linear, Jira, or another tracker.

## Required operations

The project's `docs/agents/issue-tracker.md` should make these operations
unambiguous for implementation-ticket graphs:

| Operation | Required meaning |
| --- | --- |
| Scope | Resolve an effort reference to exactly its implementation tickets. |
| List | Return ticket identities and stable references in that scope. |
| Fetch | Read title, body, comments, state, claim owner, and blocking edges. |
| Eligible | Decide whether a ticket may be executed by an agent. |
| Blockers | Resolve every `blocked by` edge to a stable ticket identity. |
| Satisfied | Decide whether a blocker has reached the required terminal state. |
| Claim | Acquire the ticket only if it is unclaimed and return ownership evidence. |
| Recheck | Confirm that the claim is still owned immediately before worker launch. |
| Fail/release | Record failure or release a claim without reporting completion. |
| Complete | Transition a ticket only after integration and required verification. |
| Report | Append worker and integration outcomes without replacing ticket history. |

Prefer native dependency, assignment, and state features. Use a documented body or
metadata convention only when the tracker lacks a native feature.

An assignee is a sufficient claim only when concurrent orchestrators can distinguish
ownership and acquisition is exclusive. If two runs use the same account, add a
run-specific token or use a stronger lock. If the backend cannot compare-and-set,
serialize acquisition behind a documented scheduler lock and state whether the lock
is process-local, repository-local, or shared across machines.

## Keep state vocabularies separate

Map backend strings to meanings; do not impose one universal list of labels.

- `ready-for-agent` is commonly an eligibility/triage role.
- `claimed` or an assignee represents execution ownership.
- `closed`, `done`, or `resolved` commonly satisfies a blocker.
- Worker success is not ticket completion. Completion occurs after serial integration
  and the required gates.
- `ready-for-human` may mean escalation or review in one project and something else
  in another. Consult the project mapping.

## Canonical snapshot

Normalize tracker reads to JSON before deterministic graph computation:

```json
{
  "version": 1,
  "effort": {
    "id": "checkout-redesign",
    "reference": "tracker-specific reference"
  },
  "nodes": [
    {
      "id": "01",
      "reference": ".scratch/checkout/issues/01-foundation.md",
      "title": "Create the checkout foundation",
      "state": "eligible",
      "blockedBy": []
    },
    {
      "id": "02",
      "reference": ".scratch/checkout/issues/02-payment.md",
      "title": "Accept payment",
      "state": "eligible",
      "blockedBy": ["01"]
    }
  ]
}
```

Allowed canonical states:

| State | Scheduler meaning |
| --- | --- |
| `eligible` | May enter the frontier when every blocker is resolved. |
| `claimed` | Owned by a worker or another run; do not schedule. |
| `resolved` | Integrated and verified; satisfies downstream blockers. |
| `failed` | Exhausted or awaiting intervention; do not schedule automatically. |
| `ineligible` | Open but not agent-executable; do not schedule. |

Require unique non-empty string ids, non-empty references and titles, and a
`blockedBy` array of ids. Preserve backend-specific fields under an optional
`metadata` object; never use them for core graph computation.

## Typical mappings

These are examples, not defaults:

| Backend | Edge | Claim | Satisfied blocker |
| --- | --- | --- | --- |
| Local Markdown | `Blocked by` field | atomic repository lock plus recorded status/token | recorded `resolved` state |
| GitHub | native dependency or body fallback | assignment plus run token/lock | closed issue |
| GitLab | native blocking link or body fallback | assignment plus run token/lock | closed issue |
| Linear/Jira | configured relation | configured exclusive assignment/lease | configured terminal workflow state |

If the tracker document does not define a required read operation, infer it only when
the result is read-only and verifiable. Never infer state-changing operations.
