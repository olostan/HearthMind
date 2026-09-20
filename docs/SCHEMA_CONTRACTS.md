# Schema and Contract Guidelines

## 1. Purpose

This document defines conventions for structured processor/domain contracts before concrete schemas are implemented.

## 2. General rules

Every persisted semantic record should have:
- stable logical id;
- revision where semantics can change;
- created timestamp;
- producer/processor provenance;
- schema/contract version.

## 3. Evidence references

Never copy source truth into opaque prose when a structured evidence reference is possible.

Evidence reference should support:
- media asset id;
- start/end;
- optional frame/timecode;
- optional crop/region;
- optional derived artifact id.

## 4. Confidence

Use numeric confidence only when its meaning is defined by the processor.

Optionally pair with:
- confidence class;
- calibration version;
- explanatory uncertainty.

Do not compare scores across unrelated models without calibration.

## 5. Extensible attributes

JSON/JSONB attributes are acceptable for processor-specific metadata, but core semantics should get explicit normalized fields once stable.

Avoid turning the entire domain into schemaless JSON.

## 6. Contract versioning

Breaking changes require a new semantic version.

Examples of breaking changes:
- changing meaning of confidence;
- changing timestamp interpretation;
- renaming semantic relation values;
- changing identity state behavior.

## 7. Enumerations

Prefer controlled extensible vocabularies for:
- observation types;
- relations/actions;
- entity kinds;
- episode kinds.

Unknown/new values should fail gracefully rather than corrupting processing.

## 8. Structured generative output

Runtime LLM/VLM processors should emit validated JSON matching an explicit schema.

Invalid output:
- retry with repair strategy if policy allows;
- otherwise mark processor run failed;
- never persist unvalidated prose as structured truth.

## 9. Future machine-readable schemas

Once implementation begins, place machine-readable contracts under:
```text
schemas/
  domain/
  processors/
  prompts/
```

Generated code must not become the only human-readable specification.
