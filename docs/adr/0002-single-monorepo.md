# ADR-0002: Single monorepo to start

- **Status:** Accepted
- **Date:** 2026-06-27
- **Deciders:** Jon

## Context

The project will eventually span a server, multiple client applications, and shared
documentation. We need a repository topology to begin in. During the planning phase
there is no code yet — only docs — and component boundaries are not yet settled.

## Decision

Use a single monorepo. Documentation lives under `docs/` now; all application code will
live under a single top-level `src/` folder, with subfolders for the individual apps and
shared libraries to be defined as the architecture settles (e.g. `src/server`,
`src/<client-app>`, `src/<shared-lib>`).

## Consequences

- Cross-cutting changes (a requirement touching server + client + docs) are one PR.
- Simplest possible starting point; one place for issues, CI, and history.
- If a client later needs an independent release cadence or different toolchain
  ergonomics, it can be split out — repo splits are straightforward early.
- Risk: monorepo CI/tooling complexity grows with the codebase; revisit if it bites.

## Alternatives considered

- **Polyrepo from day one** — rejected for now; coordination overhead before there's any
  code to isolate, and boundaries aren't defined yet.
- **Separate docs/planning repo** — rejected; keeping planning next to (eventual) code
  keeps a single source of truth and lets decisions live beside what they govern.
