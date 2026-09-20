# Glossary

## Evidence
Immutable source material or source record: video, audio, image, sensor event, receipt, etc.

## MediaAsset
An ingested immutable media object with normalized metadata and source lineage.

## EvidenceSegment
A temporal/spatial reference into Evidence, e.g. seconds 12–18 and an optional crop.

## Observation
A processor's direct detection/inference from evidence, with confidence and provenance.

## Track
A temporally continuous detected object/person within evidence. A Track is not automatically a known identity.

## IdentityHypothesis
A probabilistic association between a track and a known or anonymous entity.

## PetProfile
Enrollment record for a known household pet, including identity descriptors and reference appearance data.

## VisitorProfile
Optional record for a non-household person seen repeatedly, typically anonymous unless explicitly promoted.

## Entity
A persistent conceptual thing in the world model: person, object, vehicle, pet, food, location, supply, etc.

## Action
A structured temporal relation such as “person opened refrigerator” with actor/object/time/evidence.

## Episode
A coherent reconstructed real-world activity, possibly spanning multiple clips/cameras.

## Belief
A probabilistic proposition about household state, e.g. “milk is probably available.”

## StateTransition
A structured event that changes or informs a projected state.

## Pattern
A longitudinal statistical/logical relationship derived from many events.

## Insight
A family-oriented explanation, observation, or recommendation derived from beliefs/patterns.

## Correction
A user-authored semantic correction that creates a new revision/authority signal without altering evidence.

## New label correction
Correction that links an unknown track/entity to a new enrolled or newly created identity.

## Wrong label correction
Correction that reassigns a previously attributed identity to the correct identity or to unknown.

## Category correction
Correction that marks the underlying classification (for example person/pet/object) as incorrect.

## Visitor lifecycle correction
Correction that promotes or demotes a visitor between anonymous, recurring, and enrolled identity states.

## Recurring visitor
A stable but typically anonymous visitor identity retained by policy for repeated sightings.

## Processor
A versioned transformation from one HearthMind contract to another.

## ProcessorRun
One execution of a processor, recording inputs, version, configuration, timing, result, and errors.

## ModelRun
A model-backed ProcessorRun with model/prompt/inference provenance.

## Context Compiler
Subsystem that constructs the minimum task-relevant context for an LLM/VLM query.

## Household World Model
The evolving structured, uncertainty-aware projection of what is probably true in the household.

## Temporal Attention
The layer that decides where inside already-relevant evidence to spend additional compute.

## Reprocessing
Running newer/different processors over preserved historical inputs to generate new revisions.

## Local-first
Core operation and sensitive household state remain on user-owned hardware by default.

## Candidate Insight
A potential insight awaiting ranking/policy before being shown or notified.

## Golden Corpus
Curated annotated examples used to evaluate model/prompt changes.

## Semantic Revision
A new version of derived meaning that supersedes but does not erase prior inference.
