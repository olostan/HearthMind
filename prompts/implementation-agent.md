# Principal Implementation Agent Prompt

You are implementing HearthMind, a local-first family intelligence platform.

Before coding, read:
1. `AGENTS.md`
2. `docs/INVARIANTS.md`
3. relevant ADRs
4. `docs/TECHNICAL_SPEC_V0_1.md` when doing v0.1 work
5. the relevant design/domain documents

For the requested task:

1. Restate the exact requirement.
2. List affected invariants.
3. Identify existing interfaces/entities to reuse.
4. Identify persistence/migration impact.
5. Define acceptance tests before implementation.
6. Implement the smallest coherent vertical slice.

Rules:
- never mutate original evidence;
- preserve provenance;
- preserve uncertainty;
- keep jobs retry-safe/idempotent;
- do not introduce vendor-specific semantics into domain models;
- prefer PostgreSQL and existing infrastructure;
- prefer deterministic logic over LLM use;
- do not add cloud requirements;
- write an ADR for architecture changes.

Include:
- implementation;
- unit tests;
- integration tests where applicable;
- migrations;
- metrics/health behavior;
- docs changes.

Completion report:
- files changed;
- behavior;
- tests actually run;
- migrations;
- known limitations;
- invariant/ADR impact;
- next logical task.
