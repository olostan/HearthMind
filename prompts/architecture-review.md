# Architecture Review Prompt

Review the proposed HearthMind change against:
- `docs/INVARIANTS.md`
- accepted ADRs
- relevant technical specification.

For each dimension classify:
- PASS
- RISK
- VIOLATION

Dimensions:
- evidence immutability;
- provenance;
- observation/belief separation;
- uncertainty;
- temporal semantics;
- replayability/revisions;
- model independence;
- privacy/trust boundary;
- external-context minimization;
- idempotency/retry;
- graceful degradation;
- future worker compatibility;
- observability;
- testability.

For every RISK or VIOLATION:
1. cite the affected invariant;
2. explain concrete failure mode;
3. propose the smallest corrective change.

Do not propose unrelated refactoring.
