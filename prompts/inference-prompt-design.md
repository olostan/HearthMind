# Runtime Inference Prompt Design Prompt

Design a versioned HearthMind inference prompt for **<TASK>**.

Read:
- `docs/INVARIANTS.md`
- `docs/DOMAIN_MODEL.md`
- `docs/MODEL_STRATEGY.md`
- `docs/EVALUATION.md`

The runtime prompt must:
- distinguish direct observation from interpretation;
- never infer motivation unless the task explicitly requires a clearly labeled hypothesis;
- never identify a person beyond supplied/linked identity evidence;
- preserve ambiguity and unknowns;
- emit strict structured JSON;
- include evidence references for semantic claims;
- include confidence where meaningful;
- avoid fabricated precision;
- keep output model-independent.

Produce:
1. prompt id and semantic version;
2. purpose/non-goals;
3. system prompt;
4. input JSON schema;
5. output JSON schema;
6. repair/retry policy;
7. at least three normal examples;
8. at least three ambiguous/adversarial examples;
9. evaluation rubric;
10. golden-corpus additions needed;
11. privacy classification;
12. model capability assumptions.

A prompt that creates persisted semantic output must be evaluable before production promotion.
