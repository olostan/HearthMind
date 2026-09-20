# Personalization and Memory

## 1. Problem

Household history grows indefinitely. Sending all historical facts and patterns into every LLM context is inefficient, noisy, and eventually counterproductive.

HearthMind therefore separates memory by timescale and representation.

## 2. Memory hierarchy

### L1 — Current context
Highly volatile:
- who is home;
- current activities;
- current inventory beliefs;
- active tasks.

Stored in the world model and query context.

### L2 — Retrieved memory
Task-relevant history:
- selected episodes;
- recent meals;
- object history;
- evidence-backed past events.

Retrieved on demand.

### L3 — Structured long-term memory
Stable or semi-stable:
- routines;
- preferences;
- recurring sequences;
- statistical distributions;
- household vocabulary.

Stored explicitly and queryable.

### L4 — Model adaptation
Very stable priors:
- household-specific language;
- robust preferences;
- visual vocabulary;
- task-specific reasoning patterns.

May eventually be represented by LoRA/fine-tuning.

## 3. Context compiler

The context compiler should select the minimum useful state/history for a model request.

Example query: “What should we cook tonight?”

Compiled context might include:
- likely available ingredients;
- uncertain ingredients;
- recent meals;
- stable preferences;
- food likely to expire.

It should not include raw months of observations.

## 4. LoRA is not memory storage

Good LoRA candidates:
- stable household terminology;
- persistent taste preferences;
- household-specific object vocabulary;
- stable interpretation patterns.

Bad LoRA candidates:
- current milk quantity;
- current person location;
- one-time events;
- recent grocery receipt.

The database remains authoritative.

## 5. Training data

High-quality examples may come from:
- user corrections;
- verified episode labels;
- repeated stable observations;
- explicitly approved annotations.

Correction-derived examples should preserve correction type and context:
- new label correction: candidate enrollment examples for person/pet/visitor identity creation;
- wrong label correction: hard negative for mistaken identity pair + positive for corrected target when the correction resolves to another identity, or open-set/rejection signal when the correction resolves to unknown;
- category correction: negative detection/classification example for the superseded class + positive example for the corrected class when one is supplied (including not-a-person/not-a-pet artifacts);
- visitor lifecycle correction: transition example for promotion or demotion between anonymous visitor, recurring visitor, and enrolled identity states, with the corrected target state recorded explicitly.

Each candidate example should include:
- supporting evidence references;
- superseded prediction reference;
- processor/model/prompt version;
- configuration version or hash;
- ambiguity flags and confidence before/after correction.

Quality filtering should:
- deduplicate near-identical corrections from the same incident;
- down-rank singletons with high ambiguity or contradictory follow-up corrections;
- require repeated support before promoting stable priors into L4 adaptation.

Do not train on:
- unverified model outputs as ground truth;
- one-off ambiguous corrections without corroboration;
- policy-restricted biometric/identity-private data not approved for training scope.

Avoid training on unverified model outputs as if they were ground truth.

## 6. Training trigger

Nightly consolidation may:
- collect corrections;
- deduplicate;
- score quality;
- identify drift;
- build candidate datasets.

Scoring should account for:
- correction type;
- source ambiguity level;
- user agreement history;
- temporal recency;
- downstream contradiction rate.

Training triggers should require minimum quality-weighted sample thresholds per task (for example pet re-identification, visitor recognition, category disambiguation), not just raw correction count.

Actual LoRA training should occur only when enough new high-quality evidence exists.

## 7. Candidate promotion

Never auto-promote solely because training completed.

Compare candidate vs production on:
- accuracy;
- unsupported assertion rate;
- correction rate;
- context token reduction;
- latency/memory;
- domain-specific metrics.

## 8. Staleness

Stable priors can become stale.

Track:
- last supporting evidence;
- contradiction count;
- support count;
- stability score.

Adapters should be retrained or retired when household behavior changes.

## 9. Personal vs household memory

Future multi-user operation may separate:
- shared household memory;
- member-specific preferences;
- private member context.

Access control must be enforced at retrieval/context compilation time.
