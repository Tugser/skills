# Closure gates

Use in phases 7–9 to build the final closure matrix and pick the termination state. A gate is either `PASS` (backed by a directly observed fresh result or auditable artifact), `NOT PROVEN` (evidence unavailable or not fresh), or `NOT APPLICABLE` (with reason). Never record `PASS` from a claim, a memory of a previous run, or a reviewer's assertion. "Fresh" means produced after the final production-code edit.

## Gate definitions

- **G1 Bug contract** — observed vs expected behavior, authoritative source, reproduction conditions, and fact/hypothesis/assumption split recorded. *Mandatory.*
- **G2 Reproduction** — a minimized failing signal exists, or an equivalent causal signal (trace, known-bad artifact, differential behavior) with the reason reproduction is unavailable. *Mandatory; reproduction substitute must be justified.*
- **G3 Causal model** — one surviving hypothesis explains every material symptom; leading explanations were actively attacked and falsified; trigger vs causal defect vs contributing conditions distinguished. *Mandatory.*
- **G4 Impact and ownership** — explicit impact map covering callers, consumers, duplicated logic, contracts, config, persistence, tests; owning layer identified; correction classified as `LOCAL_IMPLEMENTATION_FIX` / `LOCAL_ARCHITECTURE_REPAIR` / `BROAD_ARCHITECTURE_CHANGE`. *Mandatory.*
- **G5 Fix contract attack** — fix contract written (change points, unchanged behavior, risks, verification matrix, exclusions) and challenged for alternate entry points, duplicate assumptions, ordering/races, and a smaller correct fix. *Mandatory.*
- **G6 Regression protection** — regression test `RED` on the buggy state and `GREEN` after the fix, or a documented strongest substitute (known-bad fixture, revert comparison, differential test, runtime trace) with the reason RED was not demonstrable. *Mandatory unless the defect is not feasibly test-automatable; then substitute evidence is mandatory.*
- **G7 Fresh verification** — after the final edit: original reproduction, regression test, related tests, broader suite where relevant, type/lint/build/static/migration checks where relevant, and final diff review; failures compared against the recorded baseline. *Mandatory; each item run fresh or marked `NOT PROVEN`.*
- **G8 Independent adversarial review** — read-only reviewer with the bug contract, reproduction, diff, and constraints, initially unaware of the claimed root cause; findings validated against evidence. *Conditional: mandatory for medium/high-risk or cross-module defects; otherwise `NOT APPLICABLE`. If unavailable where mandatory, record `NOT RUN`.*

## Matrix template

| Gate | Status | Evidence (fresh command/artifact) | Notes |
|------|--------|-----------------------------------|-------|
| G1 | | | |
| G2 | | | |
| G3 | | | |
| G4 | | | |
| G5 | | | |
| G6 | | | |
| G7 | | | |
| G8 | | | |

Closure gate score counts only mandatory applicable gates: `PASS / (8 - NOT_APPLICABLE)`.

## Termination mapping

- `RESOLVED` — all mandatory applicable gates `PASS`; no material uncertainty hidden; residual risk stated.
- `NOT_A_BUG` — G1 + authoritative contract shows observed equals expected.
- `NEEDS_BEHAVIOR_DECISION` — authoritative sources conflict; correct behavior cannot be safely inferred.
- `BLOCKED_BY_MISSING_EVIDENCE` — G2 or required environment/data/diagnostic access unavailable.
- `REQUIRES_ARCHITECTURAL_DECISION` — G4 classification is `BROAD_ARCHITECTURE_CHANGE`.
- `EXTERNAL_DEPENDENCY_BLOCKED` — closure requires an uncontrolled dependency.
