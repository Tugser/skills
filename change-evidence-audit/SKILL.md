---
name: change-evidence-audit
description: Audit a bug fix, feature implementation, local change set, Git range, or pull request before commit, push, or merge. Verify claims from the diff, source, dependency/call paths, tests, and executed quality gates; report whether tests are adequate, architectural or security review is needed, and remaining uncertainty. Use when asked to review changes, check test coverage for a change, assess a PR, or produce an evidence-based pre-commit/pre-push report. Never modify code, tests, Git history, remote state, or pull requests.
---

# Change Evidence Audit

Produce a read-only evidence package for a change set. Treat a passing test as
evidence for its asserted behavior, not proof of every affected behavior. Never
invent coverage, a test result, a remote CI result, or a qualification result.

## Resolve the target

Use the user-supplied target:

- No target: staged and unstaged working-tree changes.
- Git range or commits: compare the stated base and head.
- Pull request: inspect its diff and stated base; inspect remote checks only
  when available tools and the user's scope permit it.

Record the exact base, head, target files, and working-tree state. Do not
silently replace an explicit base. If no base exists, report that limitation.

Choose a mode unless the user does:

- `fast` (default): diff, impact analysis, test mapping, boundary inspection,
  and narrow relevant automated tests.
- `thorough`: all `fast` work plus documented quality gates for affected
  backend/frontend surfaces. Unavailable platform, model, remote-CI, or
  qualification checks never become local success.

## Establish the repository contract

Read applicable `AGENTS.md`/project instructions, test and security guidance,
architecture documentation, and package/test configuration before judging the
change. Follow its evidence hierarchy: running code, migrations, and automated
tests normally outrank current-state documents; ADRs are historical decisions.

Run `git status` before and after the audit. Treat pre-existing unrelated
changes as out of scope; do not alter, stage, discard, or reformat anything.

## Build impact and test evidence

Read every changed hunk and its surrounding implementation. For each behavioral
change, identify the observable behavior/invariant/failure mode, its defining
symbol and public seam, its callers/consumers/side effects, and tests that
exercise it through a relevant seam.

Prefer the repository's code knowledge graph for code discovery and impact
analysis: `search_graph`, `trace_path`, `get_code_snippet`, and `query_graph`.
Verify graph claims in source, especially for uncommitted files or a stale
index. Fall back to `rg` only for literals, configuration, non-code files, or
insufficient graph evidence.

Do not infer coverage from a nearby test. Read its setup, triggering action,
and assertions. It is relevant only if it could fail for the inspected
regression.

Map every changed behavior to exactly one status:

- `COVERED`: relevant test exists and passes in this audit.
- `EXISTS_NOT_RUN`: relevant test exists but was not run.
- `MISSING`: no relevant automated test was found.
- `UNKNOWN`: test relevance or result cannot be established.

Run the narrowest relevant test first. In `thorough` mode, run documented
quality gates for each affected surface afterwards. Record every command, exit
status, outcome, and why expected checks were skipped. Separate local tests,
remote CI, acceptance tests, and platform/production qualification. Skipped,
unavailable, flaky, or environment-bound checks are `UNKNOWN`, never passes.

## Inspect review-sensitive boundaries

Inspect only boundaries the diff reaches. Use repository rules first, then
language/framework norms where the repository is silent. Check as applicable:

- Architecture: layer direction, ownership, authority, contracts, ports,
  dependencies, transactions, workers/schedulers, and compatibility.
- API/UI: validation, auth/authorization, DTO/state ownership, accessibility,
  and schema/generated-client synchronization.
- Persistence/artifacts: migration direction, rollback/recovery, idempotency,
  concurrency, partial writes, and restart behavior.
- Security/supply chain: untrusted input/path handling, secrets, egress,
  subprocesses, pins, and fail-closed gates.
- Operations: config, observability, release/readiness claims, and the
  distinction between implemented, default, and qualified behavior.

Require a concrete boundary and evidence before requesting architectural or
human review. Do not suggest generic refactors or abstractions.

## Classify and report

Findings require an exact file/symbol/diff reference, test behavior, command
output, or project rule, plus a concrete consequence. Use the strongest result:

| Result | Use when |
| --- | --- |
| `BLOCKER` | A relevant check fails; a security/data-integrity/explicit project rule is violated; or changed behavior lacks required regression protection. |
| `REVIEW_REQUIRED` | The diff crosses a concrete architectural, compatibility, operational, or product-decision boundary requiring accountable human judgment. |
| `RISK_ACCEPTABLE` | Changed behaviors and affected boundaries have proportionate successful evidence and no material unresolved risk. |
| `UNKNOWN` | Required evidence could not be produced or interpreted, including skipped or inaccessible checks. State what resolves it. |

Lead with this report:

```markdown
# Change Evidence Audit

## Scope
- Target/base/head:
- Mode:
- Files and behavioral changes:

## Behavior-to-test evidence
| Changed behavior | Seam and impact evidence | Relevant test | Status | Evidence |
| --- | --- | --- | --- | --- |

## Verification executed
| Command | Result | Scope/notes |
| --- | --- | --- |

## Boundary review
- Architecture:
- Security/data integrity:
- API/UI/persistence/operations:

## Findings
- `[RESULT]` Finding — evidence, consequence, and smallest next action.

## Decision
- Overall: `BLOCKER` / `REVIEW_REQUIRED` / `RISK_ACCEPTABLE` / `UNKNOWN`
- Is human review required? Yes/No — evidence-based reason.
- What remains unverified:
```

Distinguish facts, inferences, and recommendations. A clean audit is not
authorization to commit, push, merge, release, or claim production qualification.
