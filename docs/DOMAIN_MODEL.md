# Domain Model

## 1. Design goals

The domain model must:
- separate evidence from interpretation;
- preserve temporal ranges;
- model uncertainty;
- support revisions and replay;
- allow cross-camera episodes;
- remain independent of ML vendors.

## 2. Primary entities

### Household
Administrative root for one household.

### FamilyMember
A known person enrolled by the household. Identity enrollment and face embeddings are separate sensitive records.

### PetProfile
A known household pet enrolled by the household.

Enrollment should include:
- display name;
- species/breed when known;
- optional distinguishing descriptors (coat pattern, markings, collar color/tag text);
- reference appearance samples/embeddings (whole-body or appearance-focused, not face-only by default);
- provenance for who enrolled/updated the profile and when.

Pet enrollment data is household-private and versioned like other identity-related semantics.

### VisitorProfile
Optional profile for a non-household person seen repeatedly.

Visitor profiles begin anonymous by default, may remain ephemeral, and may later be promoted to named enrollment only through explicit user action.

### Camera
Physical/logical camera source.

Fields may include:
- stable id;
- display name;
- source provider;
- location id;
- timezone;
- privacy policy id.

### Location
Semantic place in the household, e.g. garage, kitchen, driveway.

### CameraTopologyEdge
Possible movement path between locations/cameras with travel-time expectations.

### MediaAsset
Immutable ingested source media.

Important fields:
- id;
- source provider/source event id;
- camera id;
- original timestamp range;
- canonical timestamp range;
- duration;
- content hash;
- path/object key;
- codecs;
- media metadata;
- ingestion time.

### EvidenceSegment
A reference to a temporal/spatial subsection of an asset.

Examples:
- 14.2–18.4 seconds;
- crop region over several frames;
- audio span.

### Track
A temporally continuous detected entity within source evidence.

A Track does not imply known identity.
Track category may later be corrected (for example person → non-person artifact, pet → object).

### Observation
Direct model/detector output tied to EvidenceSegment.

Common fields:
- id;
- type;
- subject track/entity candidate;
- predicate/relation;
- object;
- confidence;
- payload;
- processor run id;
- evidence segment id.

### IdentityHypothesis
Association between a Track and a known/anonymous Entity with confidence and contributing evidence.

IdentityHypothesis applies to:
- family members;
- enrolled pets;
- anonymous or recurring visitors;
- unknown entities.

It should support multiple competing hypotheses, including unknown, until confidence is sufficient for downstream use.

### Entity
Persistent conceptual entity:
- Person;
- Visitor;
- Object;
- FoodItem/Class;
- Vehicle;
- Pet;
- Location;
- Supply;
- other extension types.

Entity identity is distinct from observations about that entity.

### Episode
A coherent real-world activity reconstructed from one or more source assets.

Fields:
- id;
- type;
- start/end;
- summary;
- confidence;
- status;
- revision;
- source episode/revision if superseding.

### EpisodeEvidence
Many-to-many links from episode revision to observations/evidence segments with role.

### Action
Structured temporal action within an episode:
- actor;
- predicate;
- object;
- start/end;
- confidence;
- evidence refs.

### Belief
Current or historical probabilistic proposition about household state.

Examples:
- person X is home;
- object Y is in kitchen;
- milk is probably available.

Fields:
- proposition type;
- subject;
- value/object;
- confidence;
- valid-from;
- valid-to (when superseded);
- projection version;
- supporting/contradicting refs.

### StateTransition
A state-changing event used by projections.

### Pattern
Longitudinal relationship with methodology/evidence.

Examples:
- arrival-time distribution;
- weekly routine;
- consumption interval.

Pattern should record:
- computation method;
- sample window;
- sample count;
- confidence/significance as appropriate;
- source query/version.

### Insight
Human-oriented interpretation or recommendation.

Required:
- type;
- text/structured content;
- confidence;
- relevance/urgency;
- provenance;
- lifecycle status (candidate/shown/dismissed/etc.).

### Correction
User-authored semantic correction.

Never mutates evidence. Produces corrected/revised semantic state.

Correction types should include:
- new label (create/link identity when no prior confident match exists);
- wrongly applied label (reassign to another identity or unknown);
- category correction (underlying person/pet/object classification was wrong);
- visitor lifecycle correction (anonymous visitor ↔ recurring visitor ↔ enrolled identity).

A correction creates a new authoritative semantic revision/event with provenance, confidence policy context, and references to superseded derived artifacts.

### ProcessorDefinition
Versioned semantic processor contract.

### ProcessorRun / ModelRun
Execution provenance:
- processor;
- model;
- prompt;
- configuration;
- inputs;
- timing;
- resource info;
- outputs;
- errors.

### PromptDefinition
Versioned prompt artifact.

### Job
Durable asynchronous work item.

## 3. Revision model

Persisted semantic entities that can change meaningfully should support revisions.

Recommended:
- stable logical id;
- monotonically increasing revision;
- created_by processor/correction;
- supersedes revision;
- status.

Do not overwrite an old semantic revision invisibly.

## 4. Confidence

Confidence is processor-specific and should not be assumed globally calibrated.

Persist:
- numeric score when available;
- optional confidence class;
- calibration/model metadata.

The UI may map scores to qualitative labels only when semantics are defined.

## 5. Time

Prefer timestamp ranges over single timestamps for actions.

Store:
- canonical UTC time;
- original/source timezone/offset where relevant;
- optional uncertainty;
- source clock metadata if needed.

## 6. Anonymous identities

Unknown people and unknown pets may receive ephemeral or persistent anonymous entity ids depending on household privacy policy.

Unknown visitors should default to anonymous/short-lived identity retention unless explicit policy allows recurring-visitor persistence.

Recurring visitors may keep a stable anonymous id without requiring named enrollment.

Do not automatically retain biometric identity for every visitor.

Do not force pet identity when re-identification confidence is weak; unknown pet is a valid output.

## 7. Belief projection

Beliefs are derived from transitions/evidence.

Example:
```text
grocery arrival observes milk
→ availability(milk)=likely

milk removed + returned
→ location(milk)=refrigerator remains likely

no evidence of quantity
→ quantity=unknown
```

Avoid unsupported numeric inventory estimates.

## 8. Deletion considerations

Future deletion must account for lineage:
- delete source media?
- invalidate derived observation?
- remove face enrollment?
- remove training example?
- recompute beliefs/patterns?

This requires explicit semantics before implementation.

## 9. Identity and correction lifecycle (people, pets, visitors)

Recommended progression:
```text
Track
  ↓
IdentityHypothesis(entity|unknown, confidence, evidence)
  ↓
Entity-linked Observation/Episode/Belief revisions
  ↓
Correction (if user disagrees)
  ↓
New semantic revisions + optional replay/reprojection
```

Pet re-identification should be based on appearance/body embeddings and context signals (size/shape, collar/tag, gait, co-occurrence), not face embeddings alone.

When multiple similar pets exist, disambiguation should combine:
- track continuity;
- camera/location/time continuity;
- distinguishing feature matches;
- negative evidence against alternatives.

When confidence remains ambiguous, keep competing hypotheses or assign unknown instead of forced attribution.

Corrections should propagate by creating new revisions for affected episodes/beliefs/pattern inputs, while preserving historical lineage and replayability.
