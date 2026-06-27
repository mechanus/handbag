# CLAUDE.md

Project context for Claude Code. Keep it concise and current; if a rule here stops
earning its keep, cut it. Canonical detail lives in the linked docs — this file points,
it does not duplicate.

## What this is

Handbag: a self-hosted, Plex-like home media server. **Phase: planning.** The repo is a
docs-as-code planning backbone — there is **no application code yet**. App code will land
under `src/` when the architecture settles (see
[ADR-0002](docs/adr/0002-single-monorepo.md)).

The reasons it exists (not "watch media" — Jellyfin already does that) are a three-layer
learning vehicle and end-to-end control/privacy. Read
[the product brief](docs/vision/product-brief.md) before making product decisions; it is
the north star everything traces back to.

## Repository layout

| Path | Purpose |
|------|---------|
| `docs/vision` | North star: brief, success criteria, non-goals |
| `docs/requirements` | Functional/non-functional reqs, personas, use cases |
| `docs/architecture` | System context, C4 diagrams, domain model |
| `docs/adr` | Numbered Architecture Decision Records (MADR-lite) |
| `docs/process` | How we work (branching, PRs, CI) |
| `docs/research` | Spikes and investigation notes |
| `.claude/skills` | Project-specific Claude Code skills |

## How we work

Full detail in [ways of working](docs/process/ways-of-working.md). The load-bearing rules:

- **Plan before code; docs-as-code.** Every requirement, decision, and diagram is
  version-controlled Markdown, changed via PRs. No external tool is the source of truth.
- **Every change is a PR — including solo work.** No direct pushes to `main` (ruleset
  enforced). The PR is the surface CI and AI review attach to.
- **Branch naming:** `type/short-kebab-description`, where `type` matches the commit type
  (e.g. `docs/vision-tweaks`, `feat/xbox-direct-play`, `chore/ci-foundations`).
- **[Conventional Commits](https://www.conventionalcommits.org/):** `type(scope): summary`.
  The **PR title becomes the squashed commit**, so it must be a valid Conventional Commit.
- **Squash-merge only.** History stays linear; merge-commit and rebase-merge are disabled.
- **PR descriptions follow the [template](.github/pull_request_template.md):** *What & why*
  (with `Closes #NN`), optional *Context* / *Review notes*, *Type*, *Checklist*. PRs opened
  via `gh pr create --body` bypass the template, so populate the same sections by hand.
- **The gate is passing checks, not human approval.** Single operator; GitHub blocks
  self-approval, so the ruleset requires 0 approvals + green CI. Don't wait for a reviewer.

## Decisions (ADRs)

Write an [ADR](docs/adr/README.md) when a choice is load-bearing and not obvious from the
code. Canonical triggers: implementation stack, streaming transport, transcoding engine,
remote-access transport, persistence, and any **build-vs-adopt** or **AI-placement** call.

## AI in this project (trust boundary)

This split is a hard rule from the brief, not a preference:

- **Build-time AI** (coding agents, automated PR review, security scanning) operates on
  source, PRs, and project artifacts — **cloud models are fine** (this agent included).
- **Runtime / in-product AI** operates on the user's media stream — **prefer local /
  self-hosted models; any cloud-model call is a trust-boundary decision requiring an ADR.**

Reuse libraries freely *unless* they compromise the trust boundary (phone-home, telemetry,
or a third party in the path to the media or its access control). Otherwise, building from
scratch is licensed because the learning is the deliverable.

## Project tracking

- Repo and issues live under the **`mechanus`** GitHub org (transferred from a personal
  account — old `sixtymage/handbag` links still redirect, but reference `mechanus`).
- **Native issue types** (org-level), not labels: Epic (orange) → Feature (purple) →
  Task (yellow), plus Bug (red). Every PR ties to a Task; Tasks nest under a Feature; the
  bootstrap Epic is #2.
- Live status: [Milestones](https://github.com/mechanus/handbag/milestones) (= phases
  M0–M3) and the [Project board](https://github.com/orgs/mechanus/projects/1).

## CI

The [`docs`](.github/workflows/docs.yml) workflow runs on every PR and is required by the
ruleset:

- **markdownlint** (config: `.markdownlint.jsonc`). MD013 line-length is off — prose is
  hand-wrapped to ~90 cols. Lists need blank lines around them (MD032); fenced code needs
  a language (MD040).
- **lychee** link check in `--offline` mode. It treats links as files, so **cross-repo
  links must be absolute URLs**, not `../../`-relative paths (those fail offline).

## Environment gotchas

- **Corporate TLS-intercepting proxy** breaks certificate verification for the npm
  registry and Python's `certifi`. `pip`, `gh`, and the browser work. Fix for Python:
  `pip install --user truststore`, then `truststore.inject_into_ssl()` to use the Windows
  trust store. **Do not** disable verification.
- **C# indentation is 2 spaces** (see `.editorconfig`), against the usual 4. Line endings
  are LF (`.gitattributes`), except `*.sln` which stays CRLF.
- Platform is Windows; the shell is PowerShell, with a Bash (POSIX) tool also available.
