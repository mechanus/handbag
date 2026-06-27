---
name: requirements-authoring
description: >-
  Interview-style requirements elicitation plus the house conventions for writing
  Handbag's requirements docs. Use whenever the user wants to elaborate, draft, spec,
  flesh out, or refine functional or non-functional requirements, work anywhere under
  docs/requirements/, turn a use case or a feature idea into requirements, or run a
  requirements/spec interview. The skill leads with probing questions one area at a
  time, challenges assumptions, and converges on EARS-style, traceable requirements
  with stable IDs. Use it even when the user just says "let's spec out feature X",
  "flesh out the requirements", or "what should this actually do" without naming the
  convention.
---

# Requirements Authoring

How requirements get elaborated and written in Handbag. Two halves: an **elicitation
loop** (how we draw the requirements out) and **writing conventions** (how we record
them). The conventions detail lives in
[references/conventions.md](references/conventions.md) — read it before writing or
editing any file under `docs/requirements/`.

## Why this exists (read first — it changes how you behave)

In this project **the learning is the deliverable**, not the document. The failure mode
to avoid is generating plausible-looking *volume* fast: a wall of enterprise-flavoured
requirements the operator skims and rubber-stamps. That produces a spec without
understanding, which is worthless here.

So the operator drives and you probe. Your job is to ask the questions that force a real
decision, surface the tradeoff the operator hasn't considered yet, and refuse to write a
requirement until it's genuinely understood and necessary. Converge on *fewer, sharper*
requirements rather than more. A short list everyone understands beats a long list nobody
trusts.

## The elicitation loop

Work one slice at a time and run this loop per slice. A "slice" is a coherent chunk —
usually a capability area (LIB, STREAM, …) scoped to one milestone (M1–M3), or a single
use case. Don't try to boil the ocean across areas in one pass.

1. **Anchor before asking.** Skim what already exists for this slice: the relevant
   [use cases](../../../docs/requirements/use-cases.md), [personas](../../../docs/requirements/personas.md),
   and the [product brief](../../../docs/vision/product-brief.md). The use cases are the
   raw material — most functional requirements fall out of "what has to be true for this
   flow to succeed?" Start there, not from a blank page.

2. **Probe in small batches.** Ask 2–4 pointed questions at a time, not a wall. Good
   probes target the things a spec usually leaves implicit: triggers and preconditions,
   the unhappy paths (what happens when the file is corrupt / the network drops / two
   people press play), boundaries ("how big a library?", "how many concurrent streams?"),
   and what "done well" actually means for the persona. One sharp question that exposes a
   real decision beats five that just collect agreement.

3. **Challenge, don't transcribe.** Don't accept the first answer and write it down. Name
   the competing approach, the edge case, the cost. This is where understanding — the
   actual deliverable — gets formed. If an answer is vague ("it should be fast"), push for
   the measurable version ("first frame within how many seconds, on what hardware?").

4. **Hold the line on what the brief already decided.** The brief settled build-vs-adopt,
   the build-time/runtime AI trust boundary, console-first clients, and the non-goals. Cite
   those decisions; don't re-litigate them. Actively flag when an answer drifts across a
   line: an enterprise-shaped requirement (multi-tenant, SLAs, horizontal scale) collides
   with single-home/single-operator; anything routing media or its access control through a
   third party collides with the trust boundary. Volume that violates a non-goal isn't
   progress — it's scope creep wearing a suit.

5. **Separate need from solution.** A requirement says *what* must be true and *why* —
   never *how*. When the conversation drifts into mechanism ("use HLS", "store it in
   SQLite"), that's the signal you've hit an **ADR trigger**, not a requirement. Note it as
   a decision to record later and pull back to the need.

6. **Restate, then confirm.** Before writing anything, play the requirement back in one
   plain sentence and get explicit agreement. This guards against the operator anchoring on
   *your* framing — the words must be theirs, confirmed, not yours, assumed.

7. **Write only on convergence.** Commit a requirement to the doc once it's atomic,
   verifiable, necessary, and traceable (see the quality bar below). If it doesn't trace to
   a persona and a brief driver, either find its justification or cut it.

## Writing the requirements

Match the conventions already in `docs/requirements/` exactly — don't invent a parallel
scheme. Full detail and templates are in
[references/conventions.md](references/conventions.md). The essentials:

- **Stable IDs.** Functional: `FR-<AREA>-NNN` (areas: LIB, META, STREAM, XCODE, CLIENT,
  AUTH, REMOTE, ADMIN). Non-functional: `NFR-<CAT>-NNN` (categories: PERF, MEDIA, SEC, OPS,
  CON). IDs are permanent: never renumber or reuse. A deleted requirement's ID is retired,
  not recycled — issues, ADRs, and tests point at these.
- **EARS phrasing.** Write requirements in the appropriate EARS pattern rather than freeform
  prose, so each one has a clear trigger and a testable response. The patterns and Handbag
  examples are in the reference file.
- **Functional vs non-functional.** Functional = what the system *does* (a behaviour you
  can trigger). Non-functional = a *quality or constraint* on how well it does it
  (performance, security/privacy, reliability, compatibility). When unsure, ask: "is this a
  behaviour, or a constraint on a behaviour?"
- **Traceability.** Every requirement records the persona(s) it serves and the brief driver
  it advances (learning, control/privacy, or a user-facing success criterion). Forward
  links — to use cases, ADRs, and the GitHub issue it graduates into — get added as they
  appear.
- **Lifecycle.** `proposed → accepted → implemented`. New requirements start `proposed`.

## The quality bar for a single requirement

Before writing one down, it should pass all of these — if it fails one, fix it or don't
write it:

- **Atomic** — one requirement, one testable claim. "Shall do X and Y" is two.
- **Verifiable** — you can describe the test that proves it's met. "Fast", "intuitive",
  "robust" aren't verifiable until quantified.
- **Necessary** — traces to a persona and a brief driver. If nothing needs it, cut it.
- **Implementation-free** — states the need, not the mechanism. Mechanism is an ADR.
- **Unambiguous** — one reading. EARS phrasing exists to enforce this.
- **Feasible** — achievable on the self-hosted, commodity-hardware constraint.

## Anti-patterns specific to this project

- **Enterprise cosplay.** Multi-tenant, five-nines SLAs, horizontal autoscaling — wrong
  project. Single home, single operator. Reject unless the brief says otherwise.
- **Gold-plating past the non-goals.** Live TV/DVR, music-server parity, photo management,
  general file management are explicit non-goals. Don't quietly spec them back in.
- **Solutioneering.** Writing the design as a requirement. If it says *how*, it's an ADR.
- **AI for its own sake.** In-product AI must earn its place as a real, used feature with a
  persona behind it — not a tech demo. Hold it to the same traceability bar as anything else.
- **Generating to look productive.** Ten vague requirements are worse than three sharp ones.
  Optimise for the operator's understanding, not line count.
