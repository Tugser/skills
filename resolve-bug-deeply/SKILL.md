---
name: resolve-bug-deeply
description: Investigate and resolve an existing software bug, regression, failed fix, or incorrect behavior through reproducible evidence, competing root-cause hypotheses, impact mapping, architecture-aware minimal change, regression testing, adversarial review, and explicit closure gates. Use when the user explicitly invokes this skill for a bug or asks for unusually deep, repeated verification of a proposed or implemented fix. Do not use for new feature development, broad refactoring, general code cleanup, or speculative architecture redesign.
---

# Resolve Bug Deeply

Treat prior diagnoses, proposed fixes, and reviewer claims as untrusted inputs until evidence supports them. Optimize for the smallest sufficient architecture-correct fix, not subjective confidence.

## Operating rules

- Do not edit production code before establishing a causal hypothesis supported by reproduction or equivalent evidence.
- Separate `FACT`, `HYPOTHESIS`, `ASSUMPTION`, and `UNKNOWN`; never silently promote one category to another.
- Preserve unrelated user changes. Record the initial worktree and test baseline before editing.
- Never expose credentials, tokens, cookies, authorization headers, or private environment values. Redact diagnostic output.
- Do not broaden the task into cleanup, framework replacement, mass renaming, folder restructuring, or unrelated refactoring.
- Make each loop produce information gain: eliminate a hypothesis, add evidence, expand the impact map, correct the causal model, remove a risk, or obtain a verification result. If repeated analysis produces none, change the investigative technique or stop with an explicit blocked state.
- Do not convert failed fix attempts into automatic refactor authority. After three materially different failed attempts, stop implementation and reconstruct the causal and ownership model.
- Distinguish a reported observation from verified closure evidence. A user report may establish the bug contract, but only a directly observed fresh result or an auditable artifact may pass a reproduction, causality, or verification gate.

## 1. Establish the bug contract

Record:

- observed and expected behavior;
- authoritative source for the expectation: acceptance criteria, public contract, tests, architecture decision, established behavior, then user assumption;
- reproduction conditions, entry point, affected flow, errors/logs, and relevant recent changes;
- facts, hypotheses, assumptions, and material unknowns;
- baseline worktree state and pre-existing test failures.

If authoritative sources conflict and the correct behavior cannot be inferred safely, stop with `NEEDS_BEHAVIOR_DECISION`.

## 2. Build the feedback loop

Prefer, in order: existing failing test, new behavioral regression test, integration reproduction, deterministic command/script, then manual reproduction when automation is impractical.

Minimize the reproduction while preserving the same failure mechanism. Confirm the signal fails for the reported defect rather than an unrelated setup problem. For intermittent failures, quantify frequency and use repeated runs, controlled scheduling, instrumentation, or stress/fuzz techniques as appropriate.

If reproduction is unavailable, gather an equivalent causal signal from traces, logs, a known-bad artifact, differential behavior, or history. Do not modify production code merely because one line looks suspicious.

## 3. Prove the causal model

Trace backward from the failure through callers, data/state transitions, lifecycle, configuration, persistence, caches, async boundaries, serialization, retries, and external dependencies as relevant.

Maintain 3–5 plausible hypotheses when the search space permits. For each, record supporting evidence, contradicting evidence, a falsification experiment, and implicated locations. Actively attempt to disprove the leading explanation.

Express the selected model as needed:

`trigger -> causal defect -> contributing conditions -> latent architecture weakness -> observed failure`

The model must explain all material symptoms. Distinguish the root causal defect from triggers and contributing conditions.

## 4. Map impact and ownership

Search callers, consumers, implementations, interfaces, adapters, sibling paths, duplicated logic, types/schemas, tests, configuration, persistence, and public contracts. Produce an explicit impact map and identify the module/layer that owns the invariant.

Classify the required correction:

- `LOCAL_IMPLEMENTATION_FIX`: correct locally.
- `LOCAL_ARCHITECTURE_REPAIR`: make only the structural change necessary to remove the cause or restore ownership.
- `BROAD_ARCHITECTURE_CHANGE`: stop with `REQUIRES_ARCHITECTURAL_DECISION`; do not start the redesign without separate user approval.

Load [risk-lenses.md](references/risk-lenses.md) only for domains touched by the defect.

## 5. Define and attack the fix contract

Before implementation, write a compact fix contract containing:

- causal defect and evidence;
- exact change points and ownership reasoning;
- behavior that must remain unchanged;
- regression and compatibility risks;
- a claim-to-check verification matrix;
- scope exclusions.

Assume the plan is wrong. Search for alternate entry points, duplicate assumptions, invalid-state ingress, contract violations, ordering/race/cache/persistence effects, backward-compatibility risks, and a smaller correct fix. Revise and repeat only when the challenge yields new evidence.

## 6. Implement with regression protection

When practical, create a public-behavior regression test before changing production behavior. Prove the same test is `RED` on the buggy state and `GREEN` after the fix. Derive expected values independently from the implementation under test.

If pre-fix RED cannot be demonstrated, document why and use the strongest available substitute, such as a known-bad fixture, revert/patch comparison, integration reproduction, differential test, or runtime trace. Do not falsely mark the RED gate as passed.

Implement only changes justified by the causal model. After each meaningful change, rerun the tight feedback loop and reconsider the hypothesis if evidence changes.

## 7. Verify with fresh evidence

Run fresh checks after the final edit:

1. original reproduction;
2. regression test;
3. related tests and contract checks;
4. appropriate broader suite;
5. type, lint, build, static, and migration checks when relevant;
6. final diff and impact-map review.

Compare failures against the baseline. Never claim the whole suite is clean when unrelated failures remain; state precisely what changed and what did not.

Use [closure-gates.md](references/closure-gates.md) to build the final closure matrix. Mark unavailable evidence as `NOT PROVEN`, not `PASS`.

## 8. Perform independent adversarial review

For medium/high-risk or cross-module bugs, start an independent read-only reviewer agent when subagents are available. Give it the original bug contract, reproduction, diff, tests, and architecture constraints; initially withhold the main agent's claimed root cause to reduce anchoring. Ask it to find causal gaps, missed paths, regressions, scope creep, and counterexamples. For a small local bug, one reviewer is enough; for high-risk cross-module work, use separate causal, regression/impact, and architecture/scope review passes when capacity permits.

Validate every finding against evidence. Reject unsupported reviewer claims with reasons. For a valid material finding, return to the relevant earlier phase rather than stacking a symptom patch.

If independent review is unavailable, record that gate as `NOT RUN` and do not represent it as independent validation.

## 9. Terminate honestly

Finish with exactly one state:

- `RESOLVED`: every mandatory applicable closure gate passes and no material uncertainty is hidden.
- `NOT_A_BUG`: observed behavior matches the authoritative contract.
- `NEEDS_BEHAVIOR_DECISION`: expected behavior is materially ambiguous.
- `BLOCKED_BY_MISSING_EVIDENCE`: required reproduction, environment, data, or diagnostic access is unavailable.
- `REQUIRES_ARCHITECTURAL_DECISION`: the justified repair exceeds bug scope.
- `EXTERNAL_DEPENDENCY_BLOCKED`: an uncontrolled dependency prevents closure.

Never claim mathematical certainty or “100% confidence.” Report closure completion, evidence, and residual risk. If any mandatory applicable gate is not passed, do not use `RESOLVED`.

## Final report

Report concisely:

1. termination state;
2. bug contract and causal chain;
3. impact map and architecture decision;
4. fix and preserved behavior;
5. verification matrix with fresh commands/results;
6. regression protection, adversarial findings, and residual risk;
7. closure gate score, counting only mandatory applicable gates.
