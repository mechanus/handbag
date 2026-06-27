# Handbag

> Codename. A self-hosted, Plex-like home media server: manage a media library on a
> home server and play it back on devices around the house — and, eventually, remotely
> (mobile and smart devices, e.g. while travelling).

**Status:** Planning. No application code yet — this repo is currently the planning
backbone (vision, requirements, architecture, decisions) tracked as docs-as-code.

## How we work here

- **Plan before code.** Requirements and architecture are settled and reviewed before
  implementation begins.
- **Docs-as-code.** Every requirement, decision, and diagram is version-controlled
  Markdown in this repo, changed via PRs. No external planning tool is the source of truth.
- **Decisions are recorded.** Non-trivial choices become [ADRs](docs/adr/README.md).
- **Work is tracked in GitHub.** Issues (epics/features/spikes/decisions), Milestones
  (phases), and a Project board.

## Repository layout

| Path | Purpose |
|------|---------|
| [`docs/vision`](docs/vision) | North star: what this is, what it isn't, success criteria |
| [`docs/requirements`](docs/requirements) | Functional & non-functional requirements, personas, use cases |
| [`docs/architecture`](docs/architecture) | System context, C4 diagrams, domain model |
| [`docs/adr`](docs/adr) | Numbered Architecture Decision Records |
| [`docs/process`](docs/process) | How we work: branching, PRs, CI, definition of done |
| [`docs/research`](docs/research) | Spikes and investigation notes (protocols, codecs, transport) |

This is a **monorepo**: docs today; code will be added under `src/` as the architecture settles
(see [ADR-0002](docs/adr/0002-single-monorepo.md)).

## Roadmap (phases → milestones)

| Milestone | Theme |
|-----------|-------|
| **M0** | Foundations & planning (this) |
| **M1** | LAN library & direct playback |
| **M2** | Transcoding |
| **M3** | Remote access |

See the [Milestones](../../milestones) and [Project board](../../projects) for live status.
