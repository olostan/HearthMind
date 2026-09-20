# Architecture Decision Records

HearthMind uses ADRs for decisions that materially affect architecture, semantics, privacy boundaries, or long-term maintainability.

## When to write an ADR

Write one before or alongside changes to:
- database/persistence approach;
- job transport/queue;
- domain semantics;
- service boundaries;
- evidence storage;
- provider abstraction;
- privacy trust boundaries;
- authentication/authorization architecture;
- distributed worker design;
- model/prompt promotion process;
- major language/framework decisions.

Do not write ADRs for routine implementation details.

## Numbering

Use four digits:
- `0001-...`
- `0002-...`

Never reuse numbers.

## Status

Use:
- Proposed
- Accepted
- Superseded
- Deprecated
- Rejected

## Existing baseline

The documentation currently records the intended baseline architecture. The first implementation ADRs should capture choices that become concrete during M0 rather than restating every sentence of DESIGN.md.
