# Architecture Decision Records

We record non-trivial decisions as ADRs so the *why* survives. Format is
[MADR](https://adr.github.io/madr/)-lite (see [the template](_template.md)).

## Process

1. Open a `decision` issue to debate the choice (link the options/tradeoffs).
2. When a direction is chosen, add an ADR file: `NNNN-short-title.md` (next number).
3. Submit via PR. Status starts `Proposed`; merging an accepted one sets `Accepted`.
4. Superseded decisions stay in history; mark `Status: Superseded by ADR-XXXX`.

## Index

| ADR | Title | Status |
|-----|-------|--------|
| [0001](0001-record-architecture-decisions.md) | Record architecture decisions | Accepted |
| [0002](0002-single-monorepo.md) | Single monorepo to start | Accepted |
