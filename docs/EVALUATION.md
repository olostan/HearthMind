# Evaluation Strategy

## 1. Principle

HearthMind must be measurable. “The model seems smart” is not an acceptance criterion.

Evaluation occurs at each intelligence layer.

## 2. Golden corpus

Create a small, carefully annotated local corpus early.

Initial target:
- ~100 representative clips;
- ~20 cross-camera episodes;
- known + unknown people;
- ambiguous identities;
- object interactions;
- packages/groceries/food;
- negative/noisy cases;
- several deliberately difficult temporal sequences.

Do not commit private household data to the public repository.

The repository may contain synthetic fixtures and annotation schemas.

## 3. Perception metrics

Depending on processor:
- precision/recall;
- track fragmentation;
- missed short interactions;
- face-match false accept/false reject;
- OCR accuracy;
- resource/latency.

## 4. Episode metrics

Measure:
- action correctness;
- temporal ordering;
- unsupported assertion rate;
- evidence attribution correctness;
- episode boundary quality;
- cross-camera merge precision/recall;
- identity propagation accuracy.

## 5. World-model metrics

Measure:
- belief calibration;
- stale-belief duration;
- correction propagation;
- contradictory belief incidence;
- replay determinism where expected.

## 6. Pattern metrics

Measure:
- false anomaly rate;
- pattern stability;
- sensitivity to sample size;
- seasonal/context handling;
- user acceptance where applicable.

## 7. Insight metrics

Useful product metrics:
- shown insight acceptance;
- dismissal rate;
- correction rate;
- notification opt-out;
- duplicate/repetitive insight rate;
- evidence-open rate.

## 8. Personalization metrics

For future LoRA/RAG adaptation:
- task accuracy;
- unsupported assertion rate;
- tokens/context required;
- retrieval count;
- correction rate;
- latency;
- memory footprint.

A personalized adapter is successful only if it improves defined metrics relative to the baseline.

## 9. Prompt/model promotion

Every persisted-semantic prompt/model update should compare:
- current production baseline;
- candidate;
- fixed golden corpus;
- recent drift corpus;
- resource cost.

Reject candidates that improve one metric while causing unacceptable regressions in safety/provenance/unsupported assertions.

## 10. Confidence calibration

Model confidence values are not automatically comparable.

Where confidence matters to product decisions, calibrate using held-out examples and store calibration/version metadata.

## 11. Benchmarking hardware

Record per processor:
- wall time;
- CPU time;
- peak RSS;
- GPU/iGPU utilization;
- memory bandwidth proxy if available;
- tokens/sec;
- frames/sec;
- queue throughput.

This allows the system to identify the actual future scaling bottleneck.

## 12. Regression categories

Block promotion for:
- loss of evidence references;
- increased unsupported claims;
- identity false-positive regression;
- privacy policy violation;
- schema incompatibility;
- major throughput regression without justified accuracy gain.
