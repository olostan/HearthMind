# Architectural Invariants

These invariants are stronger than implementation preferences. Violating one requires an explicit ADR and, for foundational invariants, may require revisiting the product philosophy itself.

## I-1 Evidence is immutable

Original media and source records are append-only. Processing never edits source evidence.

## I-2 Derived information has provenance

Every persisted Observation, Episode, Belief, Pattern, Insight, correction-derived revision, and model result must be traceable to:
- input evidence/derived revision;
- processor;
- processor/model version;
- relevant configuration.

## I-3 Observation is not fact

A model detector output is an Observation with uncertainty. Application code must not silently promote it to unquestionable truth.

## I-4 Belief is distinct from observation

“What is probably true now?” is a projection over historical evidence and observations. Beliefs may change without any historical observation being destroyed.

## I-5 Identity is probabilistic

Identity must support uncertainty, conflicting evidence, and unknown people. Low-confidence identity may not be silently converted into a named family member.

## I-6 Time is explicit

Every temporal artifact must have:
- canonical time/range;
- source association;
- known uncertainty when relevant.

Ordering and causality are not interchangeable.

## I-7 Corrections are semantic events

A user correction creates a new authoritative correction/revision event. It does not erase the previous model output or rewrite evidence.

## I-8 Reprocessing creates new revisions

New model output must be revisioned. Historical inference lineage remains inspectable.

## I-9 Derived state is replayable

The system must be able to reconstruct projections from preserved evidence/events using declared versions/configuration, subject to explicitly documented nondeterministic model behavior.

## I-10 Jobs are idempotent

Retrying a job with the same semantic idempotency key must not create duplicate logical outputs or corrupt state.

## I-11 Failure degrades gracefully

Failure of high-level semantic reasoning must not invalidate lower layers. Evidence remains preserved and work remains retryable.

## I-12 Domain semantics are model-independent

Core domain objects may reference generic model metadata but may not encode one provider's request/response format as the domain model.

## I-13 Cloud is optional

Core ingestion, evidence retention, timeline construction, and basic local intelligence must not require a cloud AI provider.

## I-14 Privacy policy precedes dispatch

Before evidence/context crosses a trust boundary, policy is evaluated. “Send first, redact later” is forbidden.

## I-15 External context is minimized

Cloud providers receive only task-required context. Whole household histories or raw databases are never forwarded merely for convenience.

## I-16 World model outranks model memory

LoRA/fine-tuning, embeddings, and LLM priors are derived acceleration/personalization mechanisms. They are not authoritative household state.

## I-17 Stable and volatile knowledge are separated

Rapidly changing facts belong in state/history. Stable patterns/preferences may be distilled into adapters only after evidence of stability.

## I-18 Significant insights remain explainable

The user must be able to obtain a provenance path from a significant insight back toward the observations/evidence supporting it.

## I-19 No invented precision

Do not represent uncertain physical quantities more precisely than evidence supports.

## I-20 Central authoritative state, initially

v0.x may distribute compute but not authoritative world-model ownership. Multi-primary state and consensus protocols are out of scope until justified by a later ADR.

## I-21 Processor output contracts are versioned

A semantic change to processor output requires a contract/version change or explicit migration strategy.

## I-22 Prompts are versioned

Prompts that influence persisted semantics are versioned, testable artifacts.

## I-23 Privacy boundaries are part of semantics

Camera/zone/person/provider privacy policy must be enforceable by the processing system, not only UI documentation.

## I-24 Deletion semantics are explicit

When deletion is implemented, it must distinguish:
- source evidence deletion;
- derived artifact invalidation;
- identity/biometric deletion;
- model-training-data removal where feasible.

## I-25 Metrics must not leak household content

Operational telemetry should favor identifiers, counts, durations, confidence distributions, and resource measures over raw semantic content.
