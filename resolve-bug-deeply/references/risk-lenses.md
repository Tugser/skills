# Risk lenses

Domain-specific risk lenses for phase 4 (impact mapping). Load only the lenses for domains the defect actually touches. Each lens is a checklist of failure modes and impact surfaces to sweep before writing the fix contract.

## Concurrency and async

- Race conditions, ordering guarantees, check-then-act sequences, and TOCTOU windows on the failing path.
- Missing or incorrect locks/transactions; lock scope wider or narrower than the invariant being protected.
- Awaiting promises, futures, or background jobs without error propagation or cancellation.
- Retry and timeout interactions: duplicate side effects, thundering herds, retry storms, idempotency keys.
- Queue/event ordering, at-least-once delivery assumptions, and poison-message handling.

## Persistence and data

- Constraint, transaction-boundary, and isolation-level assumptions the code relies on.
- Partial-write and orphaned-record windows; missing compensating actions.
- Schema drift, nullability mismatches, and migration ordering relative to deployed code versions.
- Read-modify-write cycles over stale reads; lost updates.
- Data repair needs: is a backfill, migration, or cleanup required as part of closure?

## Caching and state

- Invalidation gaps: writes that bypass or under-invalidate caches and derived state.
- Stale-while-reuse windows; TTL assumptions vs actual consistency requirements.
- Cache keys that collide across tenants, users, environments, or versions.
- In-memory state assumed fresh across process/module boundaries and reloads.

## External services and I/O

- Contract drift: timeouts, status codes, error shapes, pagination, and rate limits actually returned.
- Failure mode assumptions: what happens on partial responses, truncated bodies, or slow drips.
- Authentication and credential expiry mid-flow; token refresh races.
- Ordering and delivery guarantees of external queues, webhooks, and callbacks.

## Public contracts and APIs

- Consumers of the changed behavior beyond the obvious caller: SDKs, integrations, saved scripts, monitors.
- Backward compatibility of payloads, error codes, semantics, and defaults.
- Documented vs actual behavior; which one is the authoritative contract for this bug?
- Versioning escape hatches (feature flags, API versions) that make the fix safe to ship.

## Configuration and environment

- Defaults that differ across environments; config read once vs per-request.
- Secret rotation, feature flags, and kill switches that alter the failing path.
- Instance/region-specific state that explains "works there, fails here".

## Security

- Does the defect allow auth bypass, privilege escalation, injection, SSRF, or data exposure?
- Does the fix introduce a new attack surface (new endpoint, deserializer, eval-like path, shell use)?
- Logging and diagnostics must not leak credentials, tokens, cookies, PII, or private env values.

## Performance and resources

- The defect's complexity class: scan-in-loop, N+1 queries, unbounded fan-out, memory retention.
- Resource leaks triggered by the error path itself (connections, file handles, timers, listeners).
- Load-sensitive failures that only appear above thresholds; quantify before claiming fixed.

## UI and client state

- Server/client state divergence, hydration order, stale closures, and effect dependencies.
- Optimistic updates without rollback on failure.
- Error states surfaced to users: retry loops, double-submits, lost input on failure.

## Distributed and multi-instance

- Assumptions of single-writer, in-process state, or instantaneous propagation.
- Clock skew, idempotency, and ordering across instances.
- Rollout skew: mixed versions interacting during deployment.

## Build, deploy, and migrations

- Does the fix require a coordinated deploy order (schema before code, worker before web)?
- Rebuild/cache-busting steps needed for the fix to take effect (bundlers, CDN, containers).
- Rollback safety: is the change reversible if verification fails in production?
