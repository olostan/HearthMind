# Agent and Inference Prompt Library

This directory contains reusable prompts for development agents and, later, runtime inference.

Runtime prompts that produce persisted semantics must eventually be versioned with:
- prompt id/version;
- input schema;
- output schema;
- evaluation corpus/results;
- compatible model assumptions.

## Development workflows

- [Principal implementation agent](implementation-agent.md)
- [Repository task](repository-task.md)
- [Processor implementation](processor-task.md)
- [Architecture review](architecture-review.md)
- [Milestone decomposition](milestone-decomposition.md)

## Runtime prompt design

- [Inference prompt design](inference-prompt-design.md)

These are templates. Persisted runtime prompts should eventually live under a machine-readable/versioned prompt registry rather than being edited ad hoc.
