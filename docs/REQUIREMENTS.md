# Requirements

## 1. Purpose

This document captures product-level functional and non-functional requirements. Detailed implementation belongs in technical specifications and ADRs.

## 2. Functional requirements

### FR-1 Evidence ingestion
The system shall:
- ingest event-based recordings from Ring or manual imports;
- support multiple cameras;
- preserve original media, source timestamps, camera identity, audio, and source metadata;
- deduplicate repeated downloads;
- allow future source adapters without changing core semantics.

### FR-2 Perception
The system shall support processors capable of extracting:
- people;
- person tracks;
- faces and identity hypotheses;
- objects;
- person-object relationships;
- pose/activity cues;
- OCR/text;
- vehicle/pet/food/package cues;
- audio transcription and non-speech audio events where enabled.

### FR-3 Adaptive temporal analysis
The system shall:
- analyze already-interesting clips at a moderate baseline rate;
- increase temporal/spatial resolution around meaningful or ambiguous interactions;
- avoid feeding all source frames to expensive semantic models.

### FR-4 Temporal reconstruction
The system shall:
- order actions within a clip;
- represent action start/end ranges;
- preserve temporal uncertainty;
- identify state transitions;
- generate structured episode summaries.

### FR-5 Multi-camera fusion
The system shall:
- associate likely continuations across cameras;
- use identity, appearance, time, carried objects, topology, and travel-time constraints;
- merge related evidence into one episode without destroying per-camera lineage.

### FR-6 Household world model
The system shall maintain uncertain state for:
- people/presence;
- locations;
- objects;
- food/supplies;
- vehicles/pets;
- activities;
- relationships;
- other future household entities.

### FR-7 Historical patterns
The system shall support:
- routines;
- frequency distributions;
- trends;
- sequence patterns;
- deviations/anomalies;
- long-term changes.

### FR-8 Insights
The system shall generate candidate family-oriented insights such as:
- supply may be low;
- food may be unused;
- routine may differ from normal;
- household task may not have occurred;
- useful suggestion based on available evidence.

Insights must preserve provenance and confidence.

### FR-9 Natural-language access
Users shall be able to ask temporal and semantic questions over household history without reprocessing all source media for every query.

### FR-10 Evidence navigation
A user shall be able to navigate from a significant statement to the source video/audio interval supporting it.

### FR-11 Correction
Users shall be able to correct:
- identity;
- object classification;
- episode interpretation;
- world-state belief;
- other derived semantics.

Corrections shall be preserved as events and may trigger reprojection/re-evaluation.

### FR-12 Model/provider abstraction
The system shall allow local and optional cloud models behind common interfaces.

### FR-13 Personalization
The system shall support a memory hierarchy:
- current context;
- retrieval;
- structured long-term patterns/preferences;
- future LoRA/fine-tuning of stable priors.

### FR-14 Family-facing application
The eventual application shall expose:
- timeline;
- search/Ask;
- people/entity history;
- insights;
- evidence;
- corrections;
- privacy/configuration.

### FR-15 Reprocessing
Users/operators shall be able to reprocess historical evidence using updated processors/models while retaining lineage.

## 3. Non-functional requirements

### NFR-1 Privacy
Local-first is the default. External inference must be explicit and policy-controlled.

### NFR-2 Explainability
Important conclusions must be traceable to evidence.

### NFR-3 Uncertainty
The system must represent confidence, ambiguity, and unknown state.

### NFR-4 Asynchronous operation
Real-time completion is not required. Hours of processing delay are acceptable for normal intelligence workloads.

### NFR-5 Modest hardware
The first useful deployment must run on approximately:
- 6C/12T x86 CPU;
- 16 GB RAM minimum, 32 GB preferred;
- integrated GPU optional/useful;
- NVMe storage.

### NFR-6 Graceful degradation
Unavailable high-level models must not prevent ingestion, evidence retention, tracking, and later retry.

### NFR-7 Reliability
Jobs must be durable, idempotent, retryable, and crash-safe.

### NFR-8 Replayability
Derived state must be reproducible from preserved evidence plus versioned processors/configuration.

### NFR-9 Model independence
Core domain semantics must not require a particular model vendor.

### NFR-10 Horizontal extensibility
Future heterogeneous workers may execute inference/training jobs without distributing the authoritative world state.

### NFR-11 Low maintenance
The target user experience is appliance-like:
- automatic recovery;
- health visibility;
- bounded storage;
- model lifecycle management;
- simple upgrades.

### NFR-12 Security
Strong authentication, least privilege, encrypted transport, protected sensitive/biometric data, and minimal network exposure.

### NFR-13 Observability
The system must expose queue, latency, resource, error, model, correction, and confidence metrics.

### NFR-14 Auditability
A derived inference must record:
- processor/model;
- version;
- configuration;
- input lineage;
- execution time;
- output revision.

### NFR-15 Cost awareness
Cloud inference and compute-heavy reprocessing should be measurable and policy-controllable.

## 4. Explicit early non-goals

- real-time emergency response;
- perfect inventory counting;
- autonomous purchasing;
- generalized appliance control;
- distributed database consensus;
- tensor/model parallelism across mini-PCs;
- always-on cloud dependence;
- autonomous training promotion without evaluation.
