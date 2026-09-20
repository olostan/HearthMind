# ADR-0001: Identity and correction lifecycle for pets and visitors

- **Status:** Accepted
- **Date:** 2026-09-20
- **Owners:** HearthMind maintainers

## Context

The architecture already specified person identity hypotheses and correction semantics, but pet identity handling and visitor correction lifecycle were underspecified. This created a semantic gap across `DOMAIN_MODEL`, `REQUIREMENTS`, `PRIVACY_SECURITY`, and personalization/evaluation documents. Invariants require immutable evidence, uncertainty preservation, replayability, and local-first privacy.

## Decision drivers

- Preserve Track → IdentityHypothesis → Entity semantics across people, pets, and visitors.
- Keep unknown as a valid output when confidence is insufficient.
- Avoid forcing pet identity strategies to depend on human face embeddings.
- Keep visitor retention conservative by default.
- Make corrections first-class revision events that feed replay and learning without rewriting evidence.

## Considered options

### Option A: Reuse person-only identity semantics and leave pets/visitors implicit
Lower short-term documentation effort, but leaves ambiguous behavior for multi-pet households, visitor handling, and correction-driven learning.

### Option B: Define explicit pet/visitor identity and correction lifecycle semantics
Adds documentation complexity, but resolves ambiguity, aligns privacy defaults with data sensitivity, and makes correction propagation and learning measurable.

## Decision

Adopt Option B.

Define:
- pet enrollment via `PetProfile` with appearance-focused re-identification signals;
- optional visitor lifecycle via `VisitorProfile` and recurring visitor semantics;
- correction types (new label, wrong label, category correction, visitor lifecycle correction) as revision events;
- explicit propagation/replay expectations for downstream artifacts;
- correction-type-aware learning, observability, and evaluation metrics.

## Consequences

### Positive
- Person/pet/visitor identity semantics are aligned under one probabilistic model.
- Multi-pet ambiguity and visitor promotion are now explicitly modeled.
- Correction data becomes actionable for quality improvement with defined safeguards.
- Privacy policy now distinguishes human biometric data from pet/visitor identity-private appearance data.

### Negative / trade-offs
- More schema and processing contracts will be required at implementation time.
- Additional evaluation and observability metrics increase operational scope.

### Risks and mitigations
- Risk: overfitting from sparse corrections.  
  Mitigation: require deduplication, quality scoring, and minimum support thresholds before training promotion.
- Risk: excessive visitor retention.  
  Mitigation: short-lived defaults, explicit opt-in for longer retention, no automatic promotion to enrolled identities.

## Invariant impact

Affected invariants:
- I-1 Evidence is immutable
- I-2 Derived information has provenance
- I-5 Identity is probabilistic
- I-7 Corrections are semantic events
- I-8 Reprocessing creates new revisions
- I-9 Derived state is replayable
- I-14 Privacy policy precedes dispatch
- I-15 External context is minimized
- I-23 Privacy boundaries are part of semantics

## Privacy/security impact

This decision refines sensitive-data handling by introducing `IDENTITY_PRIVATE` data treatment for pet and recurring-visitor appearance data and reinforces conservative visitor-retention defaults. It does not broaden trust boundaries; external dispatch remains policy-gated and local-first.

## Rollback / supersession

Future ADRs may merge or replace pet/visitor profile semantics if implementation evidence shows unnecessary complexity. Supersession must preserve correction lineage and replay compatibility for historical revisions.

## Validation

Validate through:
- correction-type coverage in evaluation fixtures;
- pet identity FAR/FRR and unknown fallback calibration;
- visitor lifecycle metric trends;
- replay tests confirming correction propagation without mutating evidence.
