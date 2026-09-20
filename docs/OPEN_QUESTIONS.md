# Open Questions and Decisions Pending

This file intentionally records questions that should **not** be silently decided by implementation agents.

## Foundation / M0

### Q1 — Exact initial Linux distribution
Candidates:
- Debian stable
- Ubuntu Server LTS

Decision should prioritize hardware support, VAAPI/Vulkan compatibility, maintenance, and predictable upgrades.

### Q2 — Go database access approach
Candidates may include:
- stdlib/database/sql + generated/query layer;
- sqlc;
- another minimal approach.

Avoid heavy ORM semantics that obscure transactions or migrations.

### Q3 — Migration tool
Choose a boring, well-supported tool compatible with Go/PostgreSQL and CI.

### Q4 — Evidence addressing
Choose between:
- UUID path;
- content-addressed path;
- hybrid source-id + hash.

Must preserve deduplication and safe retention semantics.

### Q5 — Initial detector/tracker
Benchmark on reference hardware before standardizing.

Criteria:
- CPU throughput;
- recall of people/objects;
- license;
- ONNX/OpenVINO portability;
- stable output;
- future accelerator support.

### Q6 — Initial face-recognition model
Needs explicit privacy semantics and evaluation for false positive risk.

### Q7 — Authentication for single-household v0.1
Determine minimal secure local authentication before remote/mobile access.

### Q8 — Automatic Ring ingestion
v0.1 deliberately permits manual imports. Automatic Ring integration should be designed only after confirming a robust, maintainable, policy-compliant integration path.

## Episode intelligence / M2–M3

### Q9 — VLM baseline
Benchmark several small local VLMs against the HearthMind golden corpus rather than picking by general benchmark reputation.

### Q10 — Keyframe/attention algorithm
Candidates:
- deterministic state-change heuristics;
- embedding novelty;
- detector change score;
- hybrid attention scorer.

### Q11 — Cross-camera re-identification
Determine when face, appearance embeddings, topology, and carried-object continuity are sufficiently reliable for automatic merge vs candidate merge.

### Q12 — Episode merge correction UX
Need a simple way to split or merge episodes and turn corrections into evaluation/training examples.

## World model / M4+

### Q13 — Generic belief representation vs typed projections
Likely hybrid. Avoid an abstract knowledge graph until actual query patterns justify it.

### Q14 — Pattern-engine representation
Decide how statistics/routines are versioned and recomputed.

### Q15 — Retention defaults
Need safe defaults balancing privacy, disk growth, reprocessing, and family memory value.

## Personalization

### Q16 — LoRA training backend
Possible nodes:
- CPU home core for very small jobs;
- Apple MLX worker;
- NVIDIA worker;
- optional cloud accelerator.

Training orchestration should remain backend-neutral.

### Q17 — Stable-prior admission criteria
Define when an observed pattern is stable enough to become training data rather than remaining structured memory.

## Open source

### Q18 — Source license
Must be chosen before a broad public contributor/release push.

### Q19 — Model redistribution policy
Need a documented approach because model licenses differ from project source licensing.

### Q20 — Plugin trust model
Define permissions/data declarations before a third-party plugin ecosystem is enabled.
