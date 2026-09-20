# Roadmap

Milestones are capability milestones, not calendar commitments.

## Cross-cutting gates for every milestone

Before a milestone is considered complete, it should satisfy:
- invariants remain intact;
- persistent contracts are versioned;
- retry/replay behavior is specified;
- privacy impact is documented and tested;
- observability is sufficient to diagnose failures and drift;
- evaluation/fixtures exist for the new semantic layer;
- out-of-scope complexity is explicitly deferred rather than implicitly half-built.

## M0 — Foundation

Goal:
Establish the authoritative core that later intelligence layers can trust.

Deliver:
- repository/docs;
- PostgreSQL schema;
- migrations;
- evidence storage;
- operator-visible storage/retention controls or documented manual retention procedure;
- durable jobs;
- processor/model-run provenance;
- Docker Compose;
- health/metrics foundation.

Must specify:
- semantic contracts for MediaAsset, EvidenceSegment, Job, ProcessorRun, Track, and Observation;
- idempotency and retry rules;
- evidence addressing strategy;
- migration and rollback approach;
- baseline authentication/privacy posture for a single-household deployment.

Exit:
A video can enter, survive restart, and create deterministic processing work.

## M1 — Single-camera perception

Goal:
Produce a trustworthy evidence-backed single-camera timeline before attempting higher-order semantics.

Deliver:
- manual/Ring-compatible ingestion abstraction;
- multiple independently processed camera definitions, with no cross-camera fusion yet;
- frame extraction;
- person/object detection;
- tracking;
- face candidates, with known-family matching optional/stretch;
- evidence timeline UI/API.

Must specify:
- detector/tracker baseline and evaluation method;
- confidence semantics for tracks/observations;
- redaction-safe logging;
- operator workflow for retry/reprocess and failed-job inspection.

Exit:
A clip from any configured camera produces a timestamped evidence-backed timeline, while cross-camera fusion remains deferred.

## M2 — Temporal episode understanding

Goal:
Upgrade from detections to structured single-camera activity understanding without breaking provenance.

Deliver:
- attention windows;
- adaptive resampling;
- keyframe selection;
- VLM integration;
- privacy/policy gateway enforcement for any off-box semantic dispatch, or explicit local-only enforcement until that gateway exists;
- structured actions;
- evidence-linked episode revisions.

Must specify:
- action/episode schemas and versioning;
- model-prompt promotion criteria;
- trust-boundary rules for local vs off-box inference;
- unsupported-assertion limits and verifier strategy;
- when deterministic logic should be preferred over VLM reasoning.

Exit:
A user can inspect a chronological activity description and jump to evidence.

## M3 — Multi-camera fusion

Goal:
Merge related clips into one episode only when continuity is sufficiently justified.

Deliver:
- camera topology;
- cross-camera association;
- continuity scoring;
- merged episodes.

Must specify:
- false-merge risk policy;
- travel-time/topology assumptions;
- merge correction UX and replay semantics;
- evaluation corpus for cross-camera precision/recall.

Exit:
One physical activity spanning cameras becomes one episode.

## M4 — Household World Model

Goal:
Project current household state from historical evidence without rewriting history.

Deliver:
- entities;
- state transitions;
- belief projection;
- presence/location;
- provenance queries.

Must specify:
- typed beliefs vs generic propositions;
- projection/versioning semantics;
- staleness/expiry behavior;
- evidence/derived/biometric deletion semantics and recomputation behavior;
- contradiction handling and correction propagation.

Exit:
The system answers current-state questions and explains why.

## M5 — Family intelligence application

Goal:
Expose the system to non-developers without weakening privacy or explainability.

Deliver:
- Timeline;
- Ask/search;
- person/entity history;
- corrections;
- evidence viewer;
- mobile-friendly UI.

Must specify:
- user roles and authorization boundaries;
- evidence-view permissions;
- search context compilation rules;
- conservative notification and insight-display policy.

Exit:
A non-developer household member can use the system meaningfully.

## M6 — Long-term cognition

Goal:
Turn accumulated history into measured patterns and explainable candidate insights.

Deliver:
- routine statistics;
- sequence/pattern mining;
- anomaly candidates;
- daily/weekly summaries;
- insight ranking.

Must specify:
- pattern recomputation/versioning;
- false-anomaly tolerances;
- insight ranking and suppression logic;
- user-feedback loops for dismissals/corrections.

Exit:
Weeks of history produce useful evidence-backed patterns.

## M7 — Food and household inventory

Goal:
Add high-value household semantics while remaining honest about uncertainty.

Deliver:
- grocery arrival episodes;
- food/object beliefs;
- consumption/use events;
- uncertain inventory;
- meal/ingredient reasoning.

Must specify:
- supported confidence language for inventory state;
- retention policy for food-related inferences;
- limits on unsupported quantity estimation;
- provenance path for any recommendation.

Exit:
Food suggestions explicitly distinguish observed, inferred, and unknown inventory.

## M8 — Personalization

Goal:
Use corrections and stable patterns to improve quality without baking volatile state into models.

Deliver:
- correction datasets;
- training examples;
- adapter registry;
- LoRA/QLoRA experiments;
- automatic evaluation.

Must specify:
- admission criteria for stable training examples;
- privacy review for training/export workflows;
- rollback/promotion criteria for adapters;
- per-household isolation boundaries;
- an initial on-core training path that works before distributed worker infrastructure is required.

Exit:
Personalization measurably improves quality or reduces context/retrieval load.

## M9 — Distributed compute

Goal:
Attach optional external workers without distributing authoritative household state.

Deliver:
- worker registration;
- capability-aware scheduler;
- ephemeral Mac/GPU worker support;
- remote inference security.

Must specify:
- worker trust tiers (same-box, same-LAN, overlay-remote, cloud);
- which job types may leave the home core;
- encrypted transport, worker attestation/registration, and secret handling;
- data minimization for borrowed MacBook/NVIDIA nodes;
- revocation/offboarding flow for a detached worker.

Exit:
A new compatible worker can claim jobs without changing core semantics.

## M10 — Open ecosystem

Goal:
Open extension points only after contracts and privacy boundaries are stable.

Deliver:
- source adapter SDK;
- intelligence-module SDK;
- Home Assistant integration;
- external data-source contracts.

Must specify:
- extension permission model;
- network declaration and review requirements;
- reproducibility metadata for third-party processors;
- compatibility/evaluation requirements before promotion.

Exit:
Third parties can add useful sensors/modules without modifying core.

## Guiding rule

Do not pull later milestone complexity into earlier milestones unless it is required to preserve an invariant or avoid a known dead-end.
