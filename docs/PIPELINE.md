# Processing Pipeline

## 1. Goal

Extract as much useful temporal information as practical from motion-triggered household recordings while controlling expensive model use.

Ring already provides coarse event filtering. HearthMind therefore assumes most clips may matter.

## 2. Pipeline overview

```text
Ingest
  ↓
L1 Perception
  ↓
L2 Temporal Attention
  ↓
L3 Episode Reconstruction
  ↓
L4 World Model Projection
  ↓
L5 Long-Term Cognition
```

## 3. Ingest

Steps:
1. acquire source event/clip;
2. hash/deduplicate;
3. persist original;
4. extract metadata;
5. normalize time;
6. enqueue perception.

No semantic interpretation occurs here.

## 4. L1 Perception

Baseline processing may use:
- hardware video decode where available;
- object/person detection around ~5 FPS;
- tracking;
- face-candidate extraction when visible;
- OCR and specialized detectors only where relevant;
- audio extraction concurrently.

Outputs are Observations, Tracks, and candidate segments.

### Why not 1 FPS?
1 FPS can miss short hand-object interactions. It is acceptable for coarse semantic/keyframe sampling, not necessarily tracking/state transition discovery.

### Why not full FPS?
Most neighboring frames are redundant and expensive. Tracking + adaptive attention captures far more value per compute unit.

## 5. L2 Temporal Attention

Attention scores windows based on:
- state changes;
- object appearance/disappearance;
- door/fridge/cabinet transitions;
- interaction cues;
- identity opportunities;
- model disagreement;
- low confidence;
- novelty;
- user-defined zones/types.

Action:
- keep baseline;
- resample at higher FPS;
- crop at higher resolution;
- request another specialized detector;
- select frames for VLM.

## 6. L3 Episode Reconstruction

Inputs:
- tracks;
- observations;
- selected frames;
- audio/OCR;
- camera topology;
- neighboring clips;
- identity hypotheses.

Passes may include:
1. chronological action reconstruction;
2. state-change extraction;
3. domain pass (food/package/etc.);
4. consistency verifier.

Output:
- structured actions;
- episode type;
- summary;
- uncertainty;
- evidence mapping.

## 7. Cross-camera fusion

Candidate association uses:
- temporal proximity;
- camera topology/travel-time;
- known identity;
- face confidence;
- person re-identification/appearance;
- carried objects;
- direction of movement;
- activity continuity.

Fusion must prefer false negatives over aggressively merging unrelated people/events.

## 8. L4 World-model projection

Episode/state-transition processors update beliefs:
- presence;
- location;
- object state;
- inventory availability;
- activity status.

Projection is deterministic where practical and versioned.

## 9. L5 Cognition

Periodic jobs compute:
- routine distributions;
- frequencies;
- sequence patterns;
- deviations;
- candidate insights.

LLM use should usually explain/combine structured results, not replace the statistical engine.

## 10. Concurrency on one box

Potential overlap:
- decode next clip;
- CPU detector on another clip;
- audio transcription;
- one heavy VLM task.

Actual concurrency must be benchmarked because CPU, memory bandwidth, and iGPU share resources.

Prefer throughput benchmarks over assuming maximal parallelism is faster.

## 11. Initial performance budgeting

For one hour of cumulative motion-triggered footage on a Ryzen 5 PRO 5650GE-class box, a deep asynchronous pipeline may plausibly take on the order of roughly one to a few wall-clock hours depending on semantic density and VLM configuration.

This is an engineering budget, not a guarantee.

Measure separately:
- decode;
- detector;
- tracking;
- face;
- OCR;
- audio;
- VLM;
- fusion;
- projection.

The system should surface real per-household throughput so later hardware upgrades target the measured bottleneck.

## 12. Backpressure

The pipeline must tolerate backlog.

Priority classes might include:
- interactive reanalysis;
- newly ingested events;
- routine deep analysis;
- historical reprocessing;
- training/evaluation.

Backlog is healthy if:
- storage remains bounded;
- queue latency is visible;
- throughput exceeds long-term average arrival rate.

## 13. Reprocessing

A new model can enqueue:
- only low-confidence episodes;
- only a date range;
- only a camera;
- only a specific processor revision;
- all historical evidence.

Reprocessing writes new revisions and may trigger downstream reprojection.
