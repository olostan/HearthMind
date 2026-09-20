# HearthMind Product and System Design

## 1. Vision

HearthMind is a local-first family intelligence platform that converts fragmented household observations into an evolving, evidence-backed temporal world model.

It should eventually answer ordinary family questions such as:
- What happened after someone came home?
- Was a package brought inside?
- What food appears to have been eaten?
- What supplies may be low?
- What changed compared with our normal routine?
- Why does the system think that?

The system is not primarily an NVR, security alarm, or chatbot. Its core product is **long-lived household understanding with provenance**.

## 2. Product philosophy

HearthMind should optimize for:
1. memory over surveillance;
2. usefulness over novelty;
3. evidence over confident prose;
4. uncertainty over fabricated certainty;
5. local ownership;
6. correction and learning;
7. temporal understanding;
8. model/provider independence;
9. useful operation on modest hardware;
10. progressive intelligence as optional compute becomes available.

## 3. Core thesis: memory hierarchy

Long-term personal AI cannot scale by inserting all accumulated history into an ever-growing prompt.

HearthMind uses a hierarchy:

```text
Raw evidence
  ↓
Observations
  ↓
Episodes
  ↓
Beliefs / world state
  ↓
Patterns
  ↓
Insights
  ↓
(optional) distilled model adaptation
```

Different timescales use different representations.

- Seconds/minutes: observations and actions.
- Hours/days: episodes and state transitions.
- Weeks/months: routines and statistical patterns.
- Long-term stable behavior: preferences or personalized adapters.

The database/world model remains authoritative.

## 4. Initial product boundary

The long-term product target remains:

> Convert multi-camera event recordings into an evidence-backed, searchable chronological family timeline with people, objects, actions, and cross-camera episodes.

However, v0.1 is intentionally narrower. It focuses on one end-to-end capability:

> Convert a manually imported motion-triggered clip into immutable evidence, versioned observations, and an evidence-backed single-camera timeline.

v0.1 proves the semantic and operational substrate required for later milestones:
- immutable evidence retention;
- transactional ingest;
- durable asynchronous processing;
- versioned processor/model provenance;
- tracks and observations linked to source evidence;
- retry/reprocessing without duplicate logical outputs.

The following remain architectural extensions beyond v0.1:
- cross-camera episode fusion;
- VLM-based action interpretation;
- household world-model beliefs;
- Ask/search experiences;
- food inventory and recommendations;
- LoRA training and personalization;
- broad sensor integrations;
- distributed/multi-node compute.

## 5. Five-layer intelligence model

### L1 — Perception
Question: **What can we directly observe?**

Examples:
- person/object detections;
- face candidates/embeddings;
- OCR;
- pose;
- speech/audio events;
- track trajectories.

### L2 — Temporal Attention
Question: **Where should we spend more compute?**

Incoming Ring clips are already motion-filtered. Attention therefore operates *inside* relevant clips:
- detect state changes;
- identify ambiguous interactions;
- re-sample short windows at higher FPS/resolution;
- select VLM evidence frames.

### L3 — Episode Reconstruction
Question: **What happened?**

Combine:
- ordered actions;
- identity continuity;
- object continuity;
- camera topology;
- multi-camera evidence;
- VLM semantic interpretation.

Output is structured, not only prose.

### L4 — Household World Model
Question: **What is probably true now?**

Maintain uncertain projections for:
- people/presence;
- locations;
- objects;
- food/supplies;
- vehicles/pets;
- activities;
- relationships.

### L5 — Long-Term Cognition
Question: **What does history mean?**

Use:
- statistics;
- routine models;
- anomaly detection;
- sequence mining;
- LLM explanation/reasoning where appropriate.

Generate candidate insights rather than unconditional notifications.

## 6. Example cross-camera episode

```text
17:04:10 Garage
Val exits vehicle carrying bags

17:04:43 Front door
Val enters carrying bags

17:04:51 Living room
Val crosses room

17:05:05 Kitchen
Bags placed on counter

17:05:42 Kitchen
Milk and vegetables observed being unpacked
```

HearthMind should represent one “grocery arrival” episode while preserving every source segment and confidence.

## 7. Attention instead of brute-force video reasoning

Do not feed 30 FPS video directly to a VLM.

Typical strategy:
- tracking/object perception around ~5 FPS;
- coarse scene semantics around 1–2 FPS;
- VLM sees intelligently selected frames;
- ambiguous 2–5 second windows may be reprocessed at 5–10 FPS.

The goal is semantic information per unit compute, not uniform sampling.

## 8. World model as event-sourced projection

Do not merely store:

```text
milk.location = refrigerator
```

Preserve transitions:
- milk observed in grocery bag;
- milk placed in refrigerator;
- milk removed;
- milk returned.

Current belief is a projection:
- likely available;
- likely in refrigerator;
- quantity unknown.

Event sourcing enables correction, replay, model upgrades, and alternate projections.

## 9. Long-term cognition

Where deterministic/statistical computation is possible, use it.

Example:
- weekday arrival median: 15:43;
- 90% range: 15:35–15:54;
- N=31.

An LLM may explain the result in natural language, but should not fabricate the distribution.

## 10. Context compiler

Before invoking an LLM, compile the smallest task-relevant context.

For “What should we cook tonight?” a future context compiler might include:
- likely inventory;
- uncertain items;
- recent meals;
- stable food preferences;
- expiration/use-first evidence.

It should not include months of raw events.

## 11. Personalization and LoRA

Use model adaptation for stable priors:
- household vocabulary;
- stable preferences;
- household-specific visual concepts;
- stable interpretation tendencies.

Do not train volatile facts such as current inventory or current presence into LoRA.

Training pipeline:
1. collect high-quality corrections/examples;
2. filter/deduplicate;
3. train candidate adapter;
4. evaluate against fixed + recent corpora;
5. promote only if metrics improve.

## 12. Compute topology

Initial:
```text
single home core
  ├─ PostgreSQL
  ├─ evidence storage
  ├─ scheduler
  ├─ perception
  ├─ local model server
  └─ web/API
```

Future:
```text
             HearthMind Core
                   │
          capability-aware queue
        ┌──────────┼───────────┐
        ▼          ▼           ▼
    CPU worker   GPU worker   Mac/MLX worker
```

Scale jobs, not one model tensor across weak machines.

Future external workers are accelerators, not authorities:
- the home core remains the source of truth;
- remote workers claim capability-matched jobs and return versioned outputs;
- raw household context sent off-box is minimized by policy;
- privacy policy can forbid specific jobs from leaving the core entirely;
- a borrowed MacBook or GPU box may be enabled for evaluation/training only without receiving unrestricted household history.

## 13. External frontier models

External models are optional tools.

Expose narrow APIs:
- search_household_events;
- get_household_state;
- get_person_timeline;
- get_inventory_estimate;
- get_recent_meals;
- explain_belief.

Send task-minimal structured context rather than household history or raw video by default.

## 14. Family-facing product

Potential surfaces:
- Today;
- Timeline;
- Ask;
- Household;
- Inventory;
- Insights;
- Evidence;
- Settings.

Notifications should be conservative and ranked by:
- usefulness;
- confidence;
- urgency;
- novelty;
- user preference;
- annoyance cost.

## 15. Success definition

HearthMind succeeds when a family can ask ordinary questions about household life and get answers that are:
- useful;
- private;
- personalized;
- temporally aware;
- uncertainty-aware;
- evidence-backed;
- increasingly useful over time.

The product should feel like a private evolving memory/intelligence layer, not a camera app with an LLM attached.
