# HearthMind v0.1 Technical Specification

## 1. Objective

Build the smallest coherent system that proves HearthMind's central data architecture:

> immutable evidence → versioned observations → evidence-backed timeline

v0.1 does **not** need the full Household World Model, pattern engine, LoRA, or distributed workers.

## 2. User-visible outcome

A user can:
1. import a motion-triggered MP4 from a configured camera;
2. see it appear as immutable evidence;
3. wait for asynchronous perception;
4. view detected people/objects/tracks on a timeline;
5. inspect processor/model provenance;
6. click an observation and open the supporting timestamp in the source clip;
7. retry/reprocess failed or selected processing.

Stretch: known-family face matching.

## 3. Required components

### Core service (Go)
Responsibilities:
- config;
- database;
- camera/source metadata;
- MediaAsset ingestion;
- job creation;
- processor registry;
- observation persistence;
- timeline/query API;
- health/metrics endpoints.

### Worker (Python)
Responsibilities:
- probe media;
- extract frames;
- run initial person/object detector;
- create tracks;
- emit structured observations.

### PostgreSQL
Required tables/entities:
- households;
- cameras;
- media_assets;
- evidence_segments;
- processor_definitions;
- processor_runs;
- jobs;
- tracks;
- observations;
- model_definitions/models where applicable.

### Evidence filesystem
Immutable original files.

### Minimal web UI
Optional for the earliest backend milestone, required before v0.1 completion:
- assets list;
- clip player;
- observation timeline;
- job state;
- provenance details.

## 4. Ingestion contract

Input:
- file path/upload;
- camera id;
- source event id optional;
- source timestamp/timezone;
- provider metadata optional.

Behavior:
- compute content hash;
- reject/deduplicate same source event/content according to defined policy;
- copy/move into managed immutable evidence store;
- persist MediaAsset transactionally;
- enqueue perception job.

The source asset must be durably stored before a job is considered ready.

## 5. Processing contract

Initial processor: `basic-video-perception/v1`

Input:
- MediaAsset id/revision.

Output:
- Track records;
- Observation records;
- EvidenceSegments referencing timestamps/frame ranges.

Required detections:
- person;
- common objects as supported by chosen model.

Sampling:
- configurable, initial target ~5 FPS for detection/tracking;
- detector input resolution configurable.

No VLM required for v0.1.

## 6. Job semantics

Idempotency key example:
```text
basic-video-perception/v1:<media_asset_id>:<config_hash>
```

A retry must reuse or reconcile the same logical processor result rather than duplicate observations.

Use lease-based claiming:
- worker id;
- lease expiration;
- heartbeat;
- retry after expired lease.

## 7. ProcessorRun

Persist:
- processor id/version;
- config hash;
- code/build version if available;
- model id/version;
- started/finished;
- worker id;
- input refs;
- status/error;
- output revision;
- metrics summary.

## 8. Observation contract

Minimum:
```json
{
  "type": "person",
  "start_ms": 14200,
  "end_ms": 14600,
  "confidence": 0.94,
  "track_id": "...",
  "evidence_segment_id": "...",
  "processor_run_id": "...",
  "attributes": {}
}
```

Do not place human-readable VLM prose into the observation table in v0.1.

## 9. Track contract

Track is local to evidence/processor result unless explicitly linked later.

Minimum:
- track id;
- media asset;
- class;
- start/end;
- processor run;
- optional representative crop;
- optional embedding ref.

## 10. API

Suggested:
- `POST /api/v1/assets/import`
- `GET /api/v1/assets`
- `GET /api/v1/assets/{id}`
- `GET /api/v1/assets/{id}/timeline`
- `GET /api/v1/observations/{id}`
- `GET /api/v1/jobs/{id}`
- `POST /api/v1/assets/{id}/reprocess`
- `GET /healthz`
- `GET /readyz`
- `GET /metrics`

Exact naming can change without ADR; semantic contracts cannot.

## 11. Configuration

Config should include:
- database DSN via secret/env;
- evidence root;
- derived cache root;
- detector model;
- detection FPS;
- detector threshold;
- worker concurrency;
- retention (derived cache only in v0.1).

## 12. Observability

Metrics:
- ingested assets;
- duplicate assets;
- job queue depth by state/type;
- job duration;
- processing failures;
- frames processed;
- detector FPS;
- observations produced;
- peak/average resource metrics if practical.

Logs use ids, not raw household semantic content where avoidable.

## 13. Testing

### Unit
- idempotency-key generation;
- time conversion;
- deduplication;
- job state transitions;
- lease expiry;
- evidence path generation.

### Integration
- ingest fixture;
- restart between ingest and processing;
- worker claims job;
- worker crashes/retries;
- no duplicate observations after retry;
- timeline points to valid source interval.

### ML fixture
Use synthetic/public-safe fixture video.

Golden expectations can tolerate detector-version variance via ranges rather than exact counts.

## 14. Acceptance criteria

v0.1 is complete when:
- an imported clip persists across restart;
- background processing completes;
- timeline observations have provenance;
- every observation maps to source timestamp;
- retry does not duplicate logical output;
- failed jobs are visible/retryable;
- no cloud provider is required;
- the stack launches via documented Docker Compose.

## 15. Deferred

Explicitly deferred:
- Ring automatic downloader/auth;
- multi-camera episode fusion;
- VLM;
- audio understanding;
- world-model beliefs;
- natural-language search;
- pgvector;
- distributed workers;
- LoRA.
