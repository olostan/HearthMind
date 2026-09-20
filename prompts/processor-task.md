# Processor Implementation Prompt

Implement a HearthMind processor for:

**Purpose:** <PURPOSE>

**Input contract:** <INPUT>

**Output contract:** <OUTPUT>

Requirements:
- asynchronous;
- idempotent;
- retry-safe;
- evidence-preserving;
- provenance-preserving;
- versioned;
- confidence-aware;
- independently reprocessable.

Define:
- processor name/version;
- semantic idempotency key;
- input/output revisions;
- required worker capabilities;
- timeout/resource expectations;
- retryable vs permanent errors;
- model dependency/version;
- configuration hash.

Persist processor-run provenance.

Add metrics:
- execution duration;
- item/frame/token count as applicable;
- failures/retries;
- confidence distribution;
- resource metrics where practical.

Add:
- deterministic/synthetic fixture;
- unit tests;
- retry/idempotency integration test;
- documentation of known limitations.

Do not mutate source evidence.
