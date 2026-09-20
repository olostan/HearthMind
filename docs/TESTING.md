# Testing Strategy

## 1. Layers

### Unit tests
For:
- domain invariants;
- time conversion;
- idempotency;
- state projection;
- queue state transitions;
- privacy decisions;
- context compilation.

### Integration tests
For:
- PostgreSQL;
- evidence filesystem;
- worker job lifecycle;
- crash/retry behavior;
- migrations;
- model adapter contracts.

### ML evaluation tests
For:
- detection quality;
- identity;
- temporal actions;
- episode fusion;
- prompts/models.

### End-to-end tests
Synthetic/public-safe fixtures:
- ingest clip;
- process;
- query timeline;
- verify evidence links.

## 2. Determinism

Deterministic components should have exact assertions.

Model-driven components should use:
- tolerances;
- category expectations;
- semantic scoring;
- golden annotations.

Do not write brittle tests that require exact generative prose.

## 3. Failure testing

Required scenarios:
- worker process dies mid-job;
- lease expires;
- model backend unavailable;
- DB restart;
- duplicate ingest;
- malformed media;
- external provider denied by privacy policy;
- low disk space;
- partial derived-cache loss.

## 4. Migration tests

Schema migrations should run against:
- empty database;
- representative previous schema snapshot;
- rollback/recovery procedure when relevant.

## 5. Privacy tests

Include tests proving:
- denied camera data cannot reach cloud adapters;
- redaction occurs before dispatch;
- biometric data is not present in ordinary logs;
- user role restrictions are enforced when implemented.

## 6. Performance tests

Track:
- assets/hour;
- frames/sec;
- tokens/sec;
- average/95p job duration;
- memory peak;
- backlog drain rate.

Performance regression thresholds should be introduced after baseline measurements.

## 7. Golden corpus hygiene

Private household recordings must not enter the public repository.

Store:
- annotation schema;
- synthetic fixtures;
- dataset-generation tools;
- local-path conventions.

Private golden corpora remain outside Git.
