# Where to look

Pick targets from these lists. Each item is a place to try a run, not a finding. Turn a suspicion into a run: build the input or state that triggers it, run the real code, and show the output. Skip the lists that do not apply to the change.

## Correctness

Does the code do what the intent says it should?

- Edge cases: empty inputs, nil/undefined, boundary values, concurrent access
- Error handling: are errors caught, propagated, or silently swallowed?
- Off-by-one, type coercion, integer overflow, string encoding
- State management: race conditions, stale closures, dangling references
- Do both the happy path and the sad path work?
- Idempotency: what happens if this operation runs twice, or if a previous run crashed halfway?
- Concurrency: if multiple actors can touch the same mutable state (files, branches, shared data), can two of them interleave and corrupt it?

## Symptoms instead of causes

A fix can land on one symptom and leave the cause in place. Read the callers, callees and types around the change, then find another path to the same broken state.

- A guard clause that masks an invariant violation: does another caller still reach the bad state?
- Retry logic that hides a broken contract: does the call fail the same way when retries run out?
- A type cast that silences a modeling error: what value gets through the cast at runtime?

## Security

Feed the input through the real entry point and show where it lands.

- User input flowing to dangerous sinks (SQL, shell, eval, innerHTML) without sanitization
- Authentication/authorization gaps in new endpoints
- Secrets in code, logs, or error messages
- TOCTOU (time-of-check-time-of-use) in security-critical paths
