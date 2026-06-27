# ADR-0001: Record architecture decisions

- **Status:** Accepted
- **Date:** 2026-06-27
- **Deciders:** Jon

## Context

This is a large, planning-heavy project with many architectural choices made over a
long period, often with non-obvious tradeoffs. Without a durable record, the rationale
behind decisions evaporates and gets re-litigated.

## Decision

We record non-trivial architectural and design decisions as ADRs in `docs/adr`, using a
MADR-lite format, numbered sequentially, reviewed via PR.

## Consequences

- The *why* behind decisions is preserved and reviewable.
- Small overhead per decision; we reserve ADRs for choices with real tradeoffs, not
  every minor call.
- New contributors (and future-us) can reconstruct the reasoning quickly.

## Alternatives considered

- **No formal record** — rejected; rationale gets lost, exactly the failure mode here.
- **Wiki pages** — rejected; not versioned with the code, not PR-reviewed, drifts.
- **Decisions only in issues/PRs** — rejected as the primary record; threads are hard to
  navigate later. Issues are where we *debate*; ADRs are where we *record*.
