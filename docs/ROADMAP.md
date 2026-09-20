# Roadmap

Milestones are capability milestones, not calendar commitments.

## M0 — Foundation

Deliver:
- repository/docs;
- PostgreSQL schema;
- migrations;
- evidence storage;
- durable jobs;
- processor/model-run provenance;
- Docker Compose;
- health/metrics foundation.

Exit:
A video can enter, survive restart, and create deterministic processing work.

## M1 — Single-camera perception

Deliver:
- manual/Ring-compatible ingestion abstraction;
- frame extraction;
- person/object detection;
- tracking;
- initial face recognition;
- evidence timeline UI/API.

Exit:
One camera clip produces a timestamped evidence-backed timeline.

## M2 — Temporal episode understanding

Deliver:
- attention windows;
- adaptive resampling;
- keyframe selection;
- VLM integration;
- structured actions;
- evidence-linked episode revisions.

Exit:
A user can inspect a chronological activity description and jump to evidence.

## M3 — Multi-camera fusion

Deliver:
- camera topology;
- cross-camera association;
- continuity scoring;
- merged episodes.

Exit:
One physical activity spanning cameras becomes one episode.

## M4 — Household World Model

Deliver:
- entities;
- state transitions;
- belief projection;
- presence/location;
- provenance queries.

Exit:
The system answers current-state questions and explains why.

## M5 — Family intelligence application

Deliver:
- Timeline;
- Ask/search;
- person/entity history;
- corrections;
- evidence viewer;
- mobile-friendly UI.

Exit:
A non-developer household member can use the system meaningfully.

## M6 — Long-term cognition

Deliver:
- routine statistics;
- sequence/pattern mining;
- anomaly candidates;
- daily/weekly summaries;
- insight ranking.

Exit:
Weeks of history produce useful evidence-backed patterns.

## M7 — Food and household inventory

Deliver:
- grocery arrival episodes;
- food/object beliefs;
- consumption/use events;
- uncertain inventory;
- meal/ingredient reasoning.

Exit:
Food suggestions explicitly distinguish observed, inferred, and unknown inventory.

## M8 — Personalization

Deliver:
- correction datasets;
- training examples;
- adapter registry;
- LoRA/QLoRA experiments;
- automatic evaluation.

Exit:
Personalization measurably improves quality or reduces context/retrieval load.

## M9 — Distributed compute

Deliver:
- worker registration;
- capability-aware scheduler;
- ephemeral Mac/GPU worker support;
- remote inference security.

Exit:
A new compatible worker can claim jobs without changing core semantics.

## M10 — Open ecosystem

Deliver:
- source adapter SDK;
- intelligence-module SDK;
- Home Assistant integration;
- external data-source contracts.

Exit:
Third parties can add useful sensors/modules without modifying core.

## Guiding rule

Do not pull later milestone complexity into earlier milestones unless it is required to preserve an invariant or avoid a known dead-end.
