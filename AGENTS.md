# AGENTS.md

This file defines repository-wide instructions for coding agents (Codex, Antigravity, Claude Code, Cursor agents, and similar tools).

## Authority order

When instructions conflict, use this order:

1. `docs/INVARIANTS.md`
2. accepted ADRs in `docs/adr/`
3. `docs/TECHNICAL_SPEC_V0_1.md` for v0.1 work
4. `docs/DESIGN.md`
5. `docs/REQUIREMENTS.md`
6. task-specific issue/specification
7. this file
8. existing implementation convention

Do not silently reinterpret higher-authority documents.

## Before changing code

Read:
- `README.md`
- `docs/INVARIANTS.md`
- the relevant design/specification documents
- existing ADRs touching the subsystem

Then identify:
- requirements being implemented;
- invariants affected;
- persistent schema changes;
- failure/retry behavior;
- privacy implications;
- observability and tests.

For non-trivial architecture changes, write an ADR before or with the implementation.

## Core engineering rules

### Evidence and provenance
- Original evidence is immutable.
- Every derived persistent artifact must identify its inputs and processor/model/configuration version.
- Never store model prose as an authoritative fact without structured provenance and confidence.

### Semantic separation
Do not collapse these concepts:
- Evidence
- Observation
- Episode
- Entity
- Belief
- StateTransition
- Pattern
- Insight
- Correction

A detector output is an Observation, not a fact. A current-state projection is a Belief, not an Observation.

### Uncertainty
- Preserve confidence and ambiguity.
- Do not manufacture precision.
- “unknown” is a valid output.
- Do not convert an uncertain identity into a definitive family-member identity.

### Time
- Store canonical timestamps with timezone/offset semantics.
- Preserve source timestamp uncertainty when known.
- Temporal ordering must be explicit.
- Never infer causality merely from proximity in time.

### Idempotency and replay
- Jobs must be safe to retry.
- Derived processing is revisioned, not destructively overwritten.
- Historical evidence must remain reprocessable under newer processors/models.
- A processor's semantic output contract must be versioned.

### Privacy
- Local processing is the default.
- External/cloud inference requires policy authorization before dispatch.
- Send only the minimum required context externally.
- Face embeddings and household identity are sensitive data.
- Never log raw secrets, tokens, raw biometric vectors, or unnecessary household content.

### Model independence
Domain code must not depend directly on OpenAI, Gemini, Ollama, llama.cpp, MLX, or any specific model family. Provider-specific behavior belongs behind model/processor adapters.

### Infrastructure restraint
For v0.1:
- prefer PostgreSQL over adding Kafka/Redis/vector DB/etc.;
- prefer Docker Compose over Kubernetes;
- prefer one service/process boundary only when operationally useful;
- avoid distributed consensus or multi-primary state.

Introduce infrastructure only when a measured requirement justifies it.

### Deterministic before generative
Prefer:
- SQL
- tracking
- statistics
- explicit state machines
- classifiers
- rules
over LLM reasoning when they can solve the problem reliably.

Use LLM/VLM reasoning for tasks where semantic interpretation is genuinely needed.

## Processor contract

Every processor must define:

- processor name and semantic version;
- accepted input type/revision;
- output type/revision;
- deterministic idempotency key;
- model/version if applicable;
- configuration hash;
- retry classification;
- timeout/resource expectations;
- emitted metrics;
- provenance links;
- tests.

A processor must never mutate source evidence.

## Database changes

Every migration must:
- be forward-safe;
- preserve historical replayability;
- avoid lossy semantic migrations unless explicitly approved;
- include downgrade/recovery notes when practical.

Do not encode vendor-specific model payloads into core tables when a generic schema plus provider metadata is sufficient.

## Prompt changes

Prompts are versioned software artifacts.

A prompt change that can alter semantic output requires:
- version bump;
- schema compatibility check;
- evaluation against the golden corpus;
- regression review.

Prompts should request structured output and explicitly separate observation from inference.

## Testing expectations

New behavior should include:
- unit tests for deterministic logic;
- integration tests around persistence/retry when relevant;
- evaluation fixtures for ML/LLM semantics;
- negative/ambiguous cases.

Tests should include uncertainty and failure paths, not only happy paths.

## Definition of done

A task is done when:
- behavior matches specification;
- invariants remain satisfied;
- tests pass;
- migrations are included if needed;
- metrics/logging are adequate;
- docs are updated when semantics change;
- an ADR exists for architectural changes;
- known limitations are documented.

## Agent completion report

At completion report:
1. files changed;
2. behavior implemented;
3. tests/evaluations executed;
4. migrations;
5. known limitations;
6. any invariant or ADR implications;
7. suggested next task.

Do not claim tests passed unless they were actually run.
