---
name: summary-decision-brief
description: Distill a discussion, session, or design conversation into a concise, auditable decision brief covering context, constraints, options with tradeoffs, the decision and its owner, consequences, risks, and follow-ups. Use when the user asks for a decision brief, a decision summary, an ADR-style write-up, or asks to summarize what was decided after a long discussion or work session. Do not use for meeting transcripts, general note-taking, changelogs, or status updates.
---

# Summary Decision Brief

Write for a competent reader who missed the discussion and must act correctly without asking anyone. The brief is the record; the conversation is not. Optimize for decision auditability, not narrative.

## Operating rules

- Never promote a discussed option into a decision. Record as decided only what someone with authority actually decided; everything else stays open and labeled.
- Separate `DECIDED`, `TENTATIVE`, `DEFERRED`, `ESCALATED`, and `NOT_DECIDED`; never silently promote one state to another.
- Preserve rejected options and the reasons they were rejected; future readers re-propose them otherwise.
- Every follow-up needs an owner and a date. If unknown, mark `UNOWNED` or `UNDATED` explicitly rather than inventing one.
- Separate observed facts from interpretation and from recommendation.
- Never include credentials, tokens, private links, or unreleased confidential details in a brief.
- Target one page. Move exhaustive detail to an appendix only when the audience genuinely needs it.

## 1. Establish the brief contract

Record:

- audience and what they must be able to do after reading (approve, act, audit);
- the decision scope: one decision or a cluster; reversible or effectively irreversible;
- source material: discussion, documents, code, constraints, prior decisions;
- authority: who can actually make or confirm the decision;
- destination format: ADR file, PR description, chat post, document.

If decision authority cannot be identified, stop with `NEEDS_INPUT` rather than recording an unowned decision.

## 2. Extract the decision state

Go through the source material and classify every candidate decision:

- `DECIDED` — made, with decision owner and date;
- `TENTATIVE` — direction agreed, confirmation pending; state what confirmation is missing;
- `DEFERRED` — explicitly postponed; state the trigger or date for revisiting;
- `ESCALATED` — moved to someone else; state to whom;
- `NOT_DECIDED` — discussed but unresolved; state the blocking question.

A conversation with zero `DECIDED` or `TENTATIVE` entries is not a decision brief; see termination states.

## 3. Capture options and tradeoffs

For the central decision(s), reconstruct:

- options actually considered, including "do nothing";
- constraints that eliminated options (technical, budget, deadline, policy, prior decisions);
- the deciding criterion or tradeoff that separated the chosen option;
- a single-option decision, flagged as such, with the reason no alternatives were on the table.

Distinguish reasons given at decision time from post-hoc justification added later.

## 4. Draft the brief

Use [brief-template.md](references/brief-template.md). Adapt sections to the destination format, but keep the invariant: context, options with tradeoffs, decision with owner and date, consequences, risks, follow-ups, and open questions are all present or explicitly marked not applicable.

## 5. Attack the brief

Before delivery, verify against the source material:

- no decision is claimed that no one made;
- rationale reflects the actual deciding reason, not a tidier invented one;
- numbers, dates, names, file paths, and issue references are verbatim from source, not reconstructed from memory;
- a reader who skipped the discussion would not draw a wrong conclusion or take an wrong action;
- no load-bearing constraint or rejected option is missing;
- every open question has a stated path to resolution (who answers it, or what unblocks it).

Fix the brief, not the record: where the source is ambiguous, mark the ambiguity in the brief.

## 6. Check terminology and durability

- Use the codebase's and documents' own terms for systems, modules, and components; do not introduce renames.
- Prefer stable references (file paths, issue IDs, decision doc titles) over chat message links that rot.
- Date the brief and name its source window so later readers can judge freshness.

## 7. Terminate honestly

Finish with exactly one state:

- `COMPLETE`: every section grounded in source material; follow-ups owned or marked `UNOWNED`; residual ambiguity stated in the brief itself.
- `NO_DECISION_MADE`: the discussion produced no decision; deliver a discussion summary instead and say plainly that no decision brief applies.
- `NEEDS_INPUT`: decision state or authority cannot be determined from available material; state exactly what input is missing.

## Final report

Deliver the brief, then report:

1. termination state;
2. decision inventory with states and owners;
3. follow-ups with owners and dates, including `UNOWNED`/`UNDATED` items;
4. open questions and residual ambiguity;
5. source window the brief covers.
