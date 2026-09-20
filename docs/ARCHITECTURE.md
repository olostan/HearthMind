# Architecture

## 1. Architectural style

v0.1 is a **modular monolith plus ML workers**, deployed on one Linux host with Docker Compose.

Logical boundaries are stronger than deployment boundaries. We explicitly avoid premature microservices.

The architecture is:
- centralized authoritative state;
- immutable evidence;
- durable asynchronous processing;
- replaceable processors/models;
- optional future heterogeneous workers.

## 2. High-level components

```text
Source adapters
    │
    ▼
Evidence Store ────────┐
    │                  │
    ▼                  │
Job Scheduler          │
    │                  │
    ├─ Perception      │
    ├─ Attention       │
    ├─ Episode/VLM     │
    ├─ World Model     │
    └─ Cognition       │
    │                  │
    ▼                  │
PostgreSQL ◄───────────┘
    │
    ├─ API
    ├─ Search
    └─ Web UI
```

## 3. Core service responsibilities

### Core/API (Go)
Owns:
- domain entities;
- transactional persistence;
- job scheduling/claim logic;
- world-model projections;
- corrections;
- API;
- policy decisions;
- model-run/provenance records.

### ML workers (Python)
Own:
- frame extraction/vision preprocessing;
- object detection;
- face detection/embedding/matching;
- OCR;
- pose;
- audio/transcription;
- VLM adapters;
- embedding models;
- future training/evaluation.

Workers do not own authoritative household state.

### PostgreSQL
Initial system of record for:
- source metadata;
- processing jobs;
- observations;
- episodes;
- entities;
- beliefs/transitions;
- corrections;
- model/prompt registry metadata;
- search metadata;
- audit records.

PostgreSQL may also serve as the initial durable queue.

### Evidence store
Local filesystem initially, with object-storage semantics:
- immutable originals;
- content-addressable or UUID-addressed;
- derived artifacts separately managed;
- database stores references, not large video blobs.

### Local model server
May initially be llama.cpp or Ollama but must sit behind the model gateway.

## 4. Dataflow

```text
Ring/manual clip
   ↓
ingest + hash + metadata
   ↓
MediaAsset
   ↓
perception job(s)
   ↓
Observations + Tracks
   ↓
attention analysis
   ↓
selected EvidenceSegments
   ↓
episode reconstruction / VLM
   ↓
Episode revision
   ↓
world-model projector
   ↓
Beliefs / StateTransitions
   ↓
pattern engine
   ↓
Patterns / candidate Insights
```

## 5. Job model

Jobs are durable and idempotent.

States:
- PENDING
- CLAIMED
- RUNNING
- SUCCEEDED
- RETRYABLE_FAILURE
- PERMANENT_FAILURE
- CANCELLED

Required metadata:
- job type;
- semantic idempotency key;
- input refs/revisions;
- processor version;
- config hash;
- requirements/capabilities;
- priority;
- attempts;
- timestamps;
- output refs;
- error classification.

Workers use leases/heartbeats so abandoned jobs can be reclaimed.

## 6. Processor registry

Each processor definition identifies:
- semantic name;
- version;
- input contract;
- output contract;
- required capability;
- determinism expectations;
- default timeout;
- retry policy;
- model dependency.

This allows historical reprocessing and future distributed scheduling.

## 7. Model gateway

Generic interfaces:
- text generation;
- multimodal/image analysis;
- video-segment analysis;
- text embeddings;
- image embeddings;
- audio transcription;
- face embedding;
- classifier invocation.

Provider adapters may include:
- llama.cpp;
- Ollama;
- MLX;
- OpenAI;
- Gemini;
- others.

Provider response formats must not leak into core domain types.

## 8. Privacy gateway

Every external inference request passes through a policy check:
- source/camera;
- zone;
- household member;
- data class;
- provider;
- requested transformation.

The gateway may:
- allow;
- deny;
- redact;
- replace identity with anonymous token;
- downsample/crop;
- require local-only processing.

This same gateway concept should govern future off-box workers, not only cloud APIs. A same-LAN GPU box or temporary Mac/MLX worker is still a trust-boundary crossing if it is not the authoritative core host.

## 9. Search architecture

v0.1 should prefer PostgreSQL:
- indexed relational/time filters;
- full text;
- JSONB where appropriate;
- pgvector if/when semantic search is introduced.

Do not add a standalone vector database without measured need.

## 10. Scaling model

Future worker registration:
```json
{
  "worker_id": "vision-node-1",
  "capabilities": ["cuda", "vision", "qwen-vl"],
  "ram_gb": 32,
  "ephemeral": false
}
```

Jobs declare requirements rather than a hostname.

The authoritative DB/world model remains centralized until a later explicit decision.

Future multi-node operation should follow these rules:
- the home core owns authoritative state and scheduling decisions;
- workers register capabilities and trust tier;
- workers receive task-scoped inputs and return versioned outputs;
- no worker requires unrestricted direct access to all evidence by default;
- training/evaluation workers may be more restricted than perception workers when policy demands.

## 11. Failure strategy

- evidence ingest is durable before processing begins;
- each layer persists outputs/revisions;
- downstream failure does not invalidate upstream completion;
- reprocessing can start from the latest valid upstream artifact;
- a missing VLM leaves an episode candidate pending rather than losing data.

## 12. Storage lifecycle

Evidence retention policy is configurable.

Derived artifacts:
- thumbnails/crops can be regenerated;
- embeddings may be regenerated;
- old model revisions may be compacted only after audit/retention policy allows it.

Never delete source evidence merely because processing succeeded.

## 13. Deployment

Initial Docker Compose stack may contain:
- hearth-core;
- hearth-worker;
- postgres;
- model-server;
- web.

Logical modules can share processes initially. Split only for resource isolation or operational reasons.
