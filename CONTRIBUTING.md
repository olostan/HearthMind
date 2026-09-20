# Contributing to HearthMind

HearthMind is currently architecture-first. Contributions should preserve the project's semantic model and privacy guarantees.

## Before contributing

Read:
- [README.md](README.md)
- [AGENTS.md](AGENTS.md)
- [Architectural invariants](docs/INVARIANTS.md)
- [Product/system design](docs/DESIGN.md)
- [v0.1 technical specification](docs/TECHNICAL_SPEC_V0_1.md)

## Contribution principles

- Prefer small vertical slices over broad speculative frameworks.
- Do not introduce infrastructure without a measured need.
- Preserve evidence, provenance, uncertainty, and replayability.
- Make model/provider integrations replaceable.
- Add evaluation coverage for semantic changes.
- Treat privacy policy as executable behavior, not documentation only.

## Pull requests

A good pull request describes:
- the user/system behavior being changed;
- affected invariants;
- persistence/schema changes;
- failure and retry behavior;
- privacy impact;
- tests/evaluations;
- performance impact where relevant.

Architecture changes should include an ADR.

## Architecture Decision Records

Use `docs/adr/0000-template.md`.

An ADR is appropriate when changing:
- persistence model;
- queueing model;
- service boundaries;
- model-provider abstraction;
- privacy trust boundaries;
- core domain semantics;
- deployment topology;
- irreversible technology commitments.

## ML/model contributions

For a new model or prompt:
- document license and redistribution constraints;
- document supported hardware;
- pin/version the model;
- add evaluation results;
- record quantization;
- record input/output schema;
- avoid silently changing production semantics.

## Security and privacy

Never commit:
- Ring credentials;
- API keys;
- private household media;
- face embeddings;
- personally identifying evaluation data;
- production database dumps.

Use synthetic or explicitly approved test fixtures.

## Licensing

A repository license has not yet been selected. Until one is added, contributions should not assume a particular downstream redistribution model.
