# Repository Task Prompt

You are working in the HearthMind repository.

First read:
- `AGENTS.md`
- `docs/INVARIANTS.md`
- `docs/INDEX.md`
- any relevant ADR/spec.

Task:
**<TASK>**

Before implementation return a concise engineering note containing:
- relevant requirement ids/sections;
- affected invariants;
- affected domain entities;
- whether an ADR is required;
- schema/migration impact;
- privacy impact;
- acceptance tests.

Then implement only the required scope.

Constraints:
- keep the repository buildable;
- avoid speculative frameworks;
- preserve replayability/provenance;
- do not silently introduce cloud dependencies;
- do not hide semantic changes in prompts/config;
- use synthetic/public-safe fixtures.

Finish with:
- changed files;
- tests actually executed;
- behavior;
- limitations;
- next smallest useful task.
