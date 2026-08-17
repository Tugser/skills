---
name: architecture-aware-tdd-fix
description: Use when implementing a verified bug fix and the correct owning layer, public test seam, transaction or recovery boundary, or architecture-safe scope must be established before production edits.
---

# Architecture-Aware TDD Fix

Implement a causal fix where the invariant belongs and prove it through a stable
public seam. The invariant owner, test seam, and entry points are three separate
decisions; collapsing them is how transport guards and brittle tests become the
architecture.

**REQUIRED SUB-SKILL:** Use `tdd` for every RED/GREEN slice. Its rules for public
behavior tests, approved seams, independent expectations, and boundary-only mocks
apply throughout.

Use `resolve-bug-deeply` first when the causal defect is not supported by a
reproduction or equivalent evidence. Send the completed change to
`change-evidence-audit`; this skill does not represent its own implementation as
independent review.

## Entry contract

Before editing, record:

- the verified trigger → defect → failure chain;
- the authoritative expected behavior and unresolved conflicts;
- the repository worktree baseline and applicable instructions, architecture,
  ADR, testing, security, and workflow sources;
- the claimed invariant owner, public behavior seam, consumers, side effects,
  and preserved behavior.

If expected behavior conflicts, stop with `NEEDS_BEHAVIOR_DECISION`. If causal
evidence is missing, stop with `BLOCKED_BY_MISSING_EVIDENCE`. When the requested
outcome includes implementation or verification but repository/source access or
the required execution environment is absent, inaccessible, or explicitly
forbidden, also stop with `BLOCKED_BY_MISSING_EVIDENCE`; a sound proposed plan is
not an implemented fix.

## Stop states short-circuit execution

When a stop state is known before repository access or implementation, return a
compact decision record and stop. Use exactly these fields: state, reason,
owner/seam facts, missing decision or evidence, and the single next unblock action.
Do not continue into a hypothetical implementation plan, invent file paths or test
commands, expand speculative state machines, or describe proposed gates as results.

This short form applies to `NEEDS_BEHAVIOR_DECISION`,
`NEEDS_TEST_SEAM_APPROVAL`, `REQUIRES_ARCHITECTURAL_DECISION`, and
`BLOCKED_BY_MISSING_EVIDENCE`. The full execution and verification report below is
only for work performed against an accessible repository.

Use one of the defined termination states even for plan-only exercises. Do not
invent auxiliary states such as `DECISION_RECORDED_ONLY`, `READY_TO_IMPLEMENT`, or
`RELEASE_READY`; those labels obscure whether implementation evidence exists.

## Separate ownership from observation

Build this map before writing a test:

| Decision | Question |
| --- | --- |
| Invariant owner | Which layer must make invalid state impossible or restore consistency? |
| Public seam | Which stable caller-visible interface proves the behavior? |
| Entry points | Which routes, workers, jobs, or adapters consume that seam? |
| Side-effect boundary | Which transaction, filesystem, queue, network, or recovery contract is crossed? |

A service can be the approved seam while a domain value owns validation. An API
can preserve an admission/UX check without becoming policy authority. A restart
recovery seam can own convergence even when the trigger occurs during a write.

Classify the correction:

- `LOCAL_IMPLEMENTATION_FIX`: fix the established owner.
- `LOCAL_ARCHITECTURE_REPAIR`: make only the structural change required to
  restore ownership or dependency direction.
- Broad redesign: stop with `REQUIRES_ARCHITECTURAL_DECISION`.

An approved implementation plan confirms the named seam. Otherwise propose one,
explain why it observes the failure without coupling to internals, and stop with
`NEEDS_TEST_SEAM_APPROVAL`.

## Execute one vertical slice

1. Write one regression test through the approved public seam. Derive expected
   values from the contract, not the implementation.
2. Run the narrow test on the unchanged production code. RED is valid only when
   the command fails for the reported mechanism. A pass, collection/setup error,
   unrelated failure, or mocked implementation detail is not RED.
3. Record the command, exit result, and why the failure proves the regression.
4. Make the smallest production edit at the invariant or recovery owner. Add
   boundary translation only when an existing public contract requires it.
5. Re-run the same command and record GREEN. Then run affected consumers,
   risk-specific gates, and the repository-documented broader gate.
6. Review the final diff against the ownership map and preserved behavior. Start
   another RED/GREEN slice only for a separately justified behavior.

Keep cleanup and unrelated refactoring out of this loop. A necessary local
architecture repair belongs in the fix contract and receives its own behavioral
evidence; a failed patch does not create refactor authority.

## Verification ladder

Run fresh evidence in this order:

1. original reproduction or strongest causal substitute;
2. focused regression test;
3. affected entry points and sibling paths;
4. transaction/restart/idempotency/concurrency/compatibility gates implicated by
   the impact map;
5. repository-required suite, type, lint, build, migration, or contract checks;
6. final diff and worktree review.

Unavailable or skipped gates are `NOT PROVEN`. A plan, proposed command, nearby
passing test, or previous run is never completed evidence.

## Pressure traps

| Shortcut | Why it fails |
| --- | --- |
| Guard every entry point | Duplicates policy and leaves other callers unsafe. |
| Put the rule at the test seam | Observation does not establish ownership. |
| Catch and compensate after a dual write | Process death may bypass the catch; use the recovery contract. |
| Write the test after the patch | A passing-first test does not prove it detects the regression. |
| Call a proposed fix release-ready | Commands not freshly executed are not evidence. |
| Say “decision recorded” instead of blocked | If implementation was requested but cannot run, the missing environment is the blocker. |
| Broaden into cleanup | Scope is justified by the causal model, not proximity. |

## Final report

For performed implementation work, report:

1. exactly one termination state;
2. causal chain and architecture classification;
3. owner, approved seam, consumers, and side-effect boundary;
4. changed files and preserved behavior;
5. RED/GREEN and verification matrix with commands and results;
6. unavailable evidence, residual risk, and the handoff for
   `change-evidence-audit`.

Use exactly one state:

- `IMPLEMENTED_AND_VERIFIED` only when the implementation and all mandatory
  applicable gates were freshly executed successfully;
- `NEEDS_BEHAVIOR_DECISION`;
- `NEEDS_TEST_SEAM_APPROVAL`;
- `REQUIRES_ARCHITECTURAL_DECISION`;
- `BLOCKED_BY_MISSING_EVIDENCE`.

For example, if duration validity is a domain invariant, construct a valid domain
value/entity at the domain boundary, observe rejection through the approved
service seam, and verify API and worker consumers. Do not move the predicate into
the service merely because that is where the regression test enters.
