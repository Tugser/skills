# Decision brief template

Fill in from source material only. Delete a section only after marking it `NOT APPLICABLE` with a reason. Keep the brief to one page; push exhaustive detail to the appendix.

```markdown
# Decision brief: <one-line decision name>

- Date: <YYYY-MM-DD>
- Status: <DRAFT | PROPOSED | ACCEPTED | SUPERSEDED by <ref>>
- Decision owner: <name/role — who made or confirms the call>
- Brief author: <name/agent>
- Source window: <what discussion/document/time range this covers>

## Context

What situation forced a decision. Two to five sentences. Include only facts a
new reader needs; link or cite the authoritative source.

## Constraints

- <hard constraints: technical, budget, deadline, policy, prior decisions>

## Options considered

| Option | Key tradeoff | Why accepted/rejected |
|--------|--------------|-----------------------|
| A (chosen) | ... | ... |
| B | ... | rejected because ... |
| Do nothing | ... | ... |

Single-option decisions: state that no real alternative existed and why.

## Decision

<What was decided, in one or two imperative sentences.>

- State: <DECIDED | TENTATIVE (missing: ...)>
- Decided by: <name/role>, on <date>

## Consequences

- What becomes true once this ships/lands.
- What we are giving up or closing off (esp. if hard to reverse).

## Risks and mitigations

- <risk> — <mitigation or accepted-as-is>

## Follow-ups

| Action | Owner | Due |
|--------|-------|-----|
| <action> | <name/UNOWNED> | <date/UNDATED> |

## Open questions

- <question> — <who answers it or what unblocks it>

## Appendix (optional)

<source excerpts, data, or detailed analysis only the interested reader needs>
```
