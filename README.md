# HearthMind

**HearthMind is a local-first family intelligence platform.**

It turns household observations—starting with motion-triggered camera recordings—into an evidence-backed, temporal model of family life. The goal is not to build another NVR or security-camera dashboard. The goal is to build a private system that can remember what happened, reconstruct activities across cameras, maintain uncertain household state, discover long-term patterns, and surface useful family-oriented insights.

> **Your home remembers, but you control the memory.**

## Status

HearthMind is currently in the **architecture and foundation** phase. The repository is intentionally documentation-first: core semantics, invariants, privacy boundaries, evaluation strategy, and the v0.1 technical target are being fixed before implementation begins.

The first practical deployment target is a modest always-on Linux mini-PC (6C/12T x86 CPU, 16–32 GB RAM, integrated GPU, NVMe storage). Real-time processing is explicitly *not* a requirement. HearthMind prefers correctness, temporal richness, evidence, privacy, and recoverability over low latency.

## Initial product goal

The first serious HearthMind release should:

> Convert multi-camera motion recordings into an evidence-backed, searchable chronological family timeline containing identified people, objects, actions, and cross-camera episodes.

A successful early demo should be able to turn fragmented camera clips such as:

- garage: a family member exits a car carrying bags;
- front door: the same person enters;
- living room: the person crosses the room;
- kitchen: bags are placed on the counter and groceries are unpacked;

into one coherent episode, while preserving every source clip and timestamp needed to verify the conclusion.

## Core model

HearthMind separates increasingly interpretive concepts:

```text
Evidence
  ↓
Observation
  ↓
Episode
  ↓
Belief / State
  ↓
Pattern
  ↓
Insight
```

This separation is fundamental.

- **Evidence** is immutable source material.
- **Observations** are model-generated detections tied to evidence.
- **Episodes** reconstruct what happened over time, possibly across cameras.
- **Beliefs** represent what is probably true now.
- **Patterns** summarize statistically supported relationships over history.
- **Insights** are family-oriented explanations or recommendations.

The database/world model is authoritative. Model parameters, prompts, embeddings, RAG, and future LoRA adapters are derived mechanisms—not the source of truth.

## Five intelligence layers

Above ingestion, HearthMind uses five logical intelligence layers:

1. **Perception** — people, faces, objects, pose, OCR, audio, tracking.
2. **Temporal Attention** — decide which moments inside already-relevant clips deserve denser analysis.
3. **Episode Reconstruction** — reconstruct chronological actions and fuse related camera clips.
4. **Household World Model** — maintain uncertain state about people, objects, food, locations, activities, and relationships.
5. **Long-Term Cognition** — routines, changes, anomalies, trends, suggestions, and prospective reasoning.

These are logical boundaries, not a requirement to deploy five services.

## Key architectural principles

- **Local-first.** Raw household media, identity, face embeddings, and the world model stay local by default.
- **Evidence before inference.** Significant claims must be traceable back to source evidence.
- **Uncertainty is data.** “Likely,” “possible,” and “unknown” are legitimate states.
- **Temporal reasoning is first-class.** HearthMind reasons about sequences and state changes, not isolated frames.
- **Deterministic computation before LLMs.** Use SQL, statistics, tracking, and explicit algorithms when they solve the problem reliably.
- **Asynchronous by design.** Processing may take minutes or hours; backlog is acceptable.
- **Model-independent semantics.** Domain objects must not depend on one model provider.
- **Replayable.** Better models can reprocess historical evidence without destroying the old lineage.
- **Central state, disposable compute.** One authoritative home core initially; optional heterogeneous workers later.
- **Personalization without infinite context.** Current state, retrieved memory, structured long-term memory, and future model adaptation form a hierarchy.

## Repository guide

Start here:

- [Product and system design](docs/DESIGN.md)
- [Functional and non-functional requirements](docs/REQUIREMENTS.md)
- [Architectural invariants](docs/INVARIANTS.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Domain model](docs/DOMAIN_MODEL.md)
- [Processing pipeline](docs/PIPELINE.md)
- [v0.1 technical specification](docs/TECHNICAL_SPEC_V0_1.md)
- [Privacy and security](docs/PRIVACY_SECURITY.md)
- [Evaluation strategy](docs/EVALUATION.md)
- [Roadmap](docs/ROADMAP.md)
- [Deployment and hardware assumptions](docs/DEPLOYMENT.md)
- [Architecture Decision Records](docs/adr/README.md)
- [Instructions for coding agents](AGENTS.md)
- [Reusable agent prompts](prompts/README.md)

## Planned implementation shape

The current design preference is:

- **Go** for core API, scheduler, durable domain logic, world-model projection, ingestion coordination, and storage.
- **Python** for CV/ML workers, VLM adapters, face recognition, audio processing, evaluation, and training.
- **PostgreSQL** as the initial system of record and durable work queue.
- **Filesystem/object-style local storage** for immutable video/audio evidence.
- **Docker Compose** for the first single-box deployment.
- **TypeScript** for the family-facing web application.

These choices are defaults, not dogma. Changes to core semantics or infrastructure require an ADR.

## What HearthMind is not

Early HearthMind is **not** intended to be:

- a real-time alarm/security response system;
- a replacement for Home Assistant;
- an autonomous purchasing system;
- a distributed database;
- tensor-parallel inference across weak home computers;
- a system that treats every model output as fact;
- a cloud-first video-analysis service;
- an “AI watches your family” product.

## Contributing

HearthMind is still establishing its contract with future implementations. Before contributing code, read [AGENTS.md](AGENTS.md), [docs/INVARIANTS.md](docs/INVARIANTS.md), and the relevant design/specification documents.

See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution workflow.

## License

A project license has not yet been selected. Do not assume permission beyond GitHub's default repository terms until a license is added.
