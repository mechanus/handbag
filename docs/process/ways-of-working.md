# Ways of Working

> How changes get made in Handbag. The [product brief](../vision/product-brief.md) says
> *why* we work "done properly, to go faster"; this doc owns the *how*. It is
> deliberately lightweight — every rule here should either speed us up or protect the
> trust boundary. If one stops earning its keep, cut it.

## Principles (from the brief)

- **Done properly, to go faster.** Hygiene is the structure that lets AI-assisted work
  move fast safely — not bureaucracy.
- **Finish thin slices first.** Vertical, end-to-end increments over horizontal
  perfection. Limit work-in-progress.
- **Decisions are recorded.** Load-bearing choices become ADRs.

## Branching

- Trunk-based: `main` is always releasable (or, for now, always coherent).
- Work happens on **short-lived branches** off `main`, merged back via PR, then deleted
  (auto-delete on merge is on).
- Branch naming: `type/short-kebab-description`, where `type` matches the commit types
  below — e.g. `docs/vision-tweaks`, `feat/xbox-direct-play`, `chore/ci-foundations`.
- No direct pushes to `main`. Everything lands through a PR (enforced by ruleset).

## Commits

[Conventional Commits](https://www.conventionalcommits.org/): `type(scope): summary`.

| Type | Use for |
|------|---------|
| `feat` | New capability |
| `fix` | Bug fix |
| `docs` | Documentation / planning artifacts |
| `refactor` | Behaviour-preserving code change |
| `test` | Tests only |
| `build` | Build system, dependencies, packaging |
| `ci` | CI/automation config |
| `chore` | Maintenance that doesn't fit above |

Scope is optional but encouraged (`feat(transcode):`, `docs(adr):`).

## Pull requests

- **Every change is a PR**, including solo work — the PR is the surface that CI and
  AI review attach to, and self-review on a rendered diff catches real mistakes.
- Keep PRs small and focused — ideally one vertical slice.
- The **PR title is the squashed commit message**, so it must be a valid Conventional
  Commit (we squash-merge; the title becomes history).
- Link the issue(s) the PR closes (`Closes #NN`).
- Self-review checklist before requesting merge:
  - [ ] CI is green.
  - [ ] Diff reads cleanly top-to-bottom; no stray/debug content.
  - [ ] Docs/cross-links updated; new load-bearing decision captured as an ADR.
  - [ ] No secret, token, or home-network specific detail committed.

## Merging

- **Squash merge only** (merge commits and rebase-merge are disabled). History stays
  linear and bisectable; one commit per merged slice.
- CI must pass before merge (ruleset-enforced).
- We are a single operator, so PRs are **not** gated on a human approval (GitHub won't
  let you approve your own PR). The gate is *passing checks* — and, over time, an
  automated AI reviewer running as a required check.

## Continuous integration

- Today: the [`docs`](../../.github/workflows/docs.yml) workflow lints Markdown and
  verifies internal links on every PR.
- As code lands, CI grows build, unit/integration test, and packaging jobs. The
  required-checks list in the ruleset is updated to match.

## When to write an ADR

Write an [ADR](../adr/README.md) when a decision is **load-bearing and not obvious from
the code** — anything future-you would want the *reasoning* for, not just the outcome.
Canonical triggers for this project: implementation stack, streaming transport,
transcoding engine, remote-access transport, persistence, and any **build-vs-adopt** or
**AI placement** (build-time vs runtime) call per the brief's trust-boundary rules.

## AI in the workflow

- **Build-time AI** (coding agents, automated PR review, security scanning) operates on
  source and PRs — cloud models are fine, per the brief.
- **Runtime / in-product AI** operates on the user's media — prefer local/self-hosted
  models; any cloud call is a trust-boundary decision requiring an ADR.

## Deferred until code lands

Tracked here so they're not forgotten, intentionally **not** set up yet:

- Pre-commit hooks (format/lint on commit).
- Test + coverage gates wired into CI.
- Dependabot / dependency-update automation.
- CodeQL / SAST as a required check.
- Automated AI PR review as a required check.
- Release, versioning, and packaging pipeline.
