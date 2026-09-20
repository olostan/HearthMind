# Observability

## 1. Goals

Observability should answer:
- Is the pipeline healthy?
- Where is backlog accumulating?
- Which processor dominates compute?
- Which model versions are producing errors/corrections?
- Are confidence distributions drifting?
- Is privacy-sensitive data leaking into telemetry?

## 2. Metrics

### Ingestion
- assets ingested;
- duplicates;
- bytes/minute;
- ingest failures.

### Queue
- depth by job type/state/priority;
- oldest pending age;
- lease expirations;
- retry counts;
- permanent failures.

### Processor
- duration histogram;
- throughput;
- input/output counts;
- model load time;
- frames/tokens processed;
- confidence distribution.

### Resources
- CPU;
- RSS;
- disk;
- I/O;
- GPU/iGPU utilization when available;
- model memory;
- thermal throttling if practical.

### Semantic quality
- user corrections;
- correction count by type (new label, wrong label, category, visitor lifecycle);
- identity corrections;
- pet identity false-accept/false-reject trend;
- unknown-identity fallback rate by class (person/pet/visitor);
- recurring-visitor creation/promotion/demotion rate;
- unsupported-claim evaluation rate;
- episode merge corrections;
- insight dismissals.

## 3. Tracing

Use correlation ids across:
- asset;
- job;
- processor run;
- episode;
- downstream projection.

A user-visible episode should be traceable through logs without logging household content itself.

## 4. Logging

Prefer:
- ids;
- counts;
- durations;
- processor/model versions;
- error codes.

Avoid:
- raw transcripts;
- face vectors;
- full prompts containing household history;
- raw media paths exposed externally;
- secrets.

## 5. Health

Expose:
- liveness;
- readiness;
- DB connectivity;
- evidence-store writability;
- worker heartbeat;
- model backend readiness.

## 6. Capacity planning

Generate periodic compute profiles:
```text
decode          4%
detector       12%
face            5%
audio          10%
VLM            58%
projection      3%
patterns        8%
```

These profiles should guide future hardware purchases rather than speculation.
