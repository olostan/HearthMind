# Milestone Decomposition Prompt

Using the authoritative HearthMind documentation, decompose milestone **<MILESTONE>** into implementation epics and tasks.

For each task provide:
- objective;
- user/system value;
- prerequisites;
- affected domain entities;
- schema/API changes;
- processor/model changes;
- privacy implications;
- acceptance criteria;
- automated tests;
- observability;
- explicit non-goals.

Order tasks so:
- every intermediate state builds/runs;
- each epic ends with a usable vertical slice;
- later-milestone complexity is not pulled forward unless required by an invariant.

Flag any requirement that needs an ADR before implementation.
