# Product Brief / Vision

> The north star. Everything else (requirements, architecture, roadmap) should trace
> back to this. Keep it short and opinionated.

## One-line pitch

_A self-hosted home media server you own end to end — built to be understood, and to
keep your library and how you reach it entirely under your own roof._

## The problem

Let's be honest about the baseline: **Jellyfin already solves "I want to watch my
media."** It's mature, free, actively developed, has the clients, does LAN playback and
transcoding today, and has workable remote-access stories. Measured purely on utility or
time-to-watch, `docker run jellyfin` beats building Handbag. If watching media were the
only goal, this project would be procrastination with extra steps.

It exists for two reasons that Jellyfin does **not** give you:

1. **Learning vehicle (three layers).** Building the interesting parts yourself is the
   point, not a means to an end. Reimplementing something that already exists is a
   *feature* here, because the understanding is the deliverable. The learning spans
   three layers:

   - **The solution** — the media server itself: transcoding pipeline, streaming
     transport, distributed playback across the house, secure remote access.
   - **AI mastery** — taking AI use beyond day-job competence, in two distinct modes:
     - *AI in the making* — coding-agent skills/subagents tuned for this codebase,
       design tooling for the web/app UI (e.g. Claude-assisted design), and AI woven
       into the toolchain (automated PR review, security scanning, release notes).
     - *AI in the product* — custom model use as a real feature, e.g. real-time
       contextual subtitle translation for stretches of dialogue in a language other
       than the one principally in use.
   - **Engineering craft** — a quasi-commercial-grade SDLC on a home project, on
     purpose: a planned roadmap, work broken down into epics → features → stories,
     and solid build, testing, packaging, and release. Not ceremony — this is the
     *organised means by which AI helps us go faster*, and the thing that keeps the
     other two layers from collapsing under their own ambition.

2. **Control / privacy.** Owning the trust boundary end to end: no phone-home, no
   telemetry, no third party sitting in the path between you and your own library —
   especially for remote access. The differentiator isn't a feature Jellyfin lacks;
   it's *who you have to trust* to use it.

This is explicitly **not** a claim that Jellyfin is missing a feature we need. If a
feature gap ever becomes the real reason to build something, we name it here.

And one more thing, stated plainly because it's true and it shapes decisions:
**this should be enormous fun.** Done this way — properly, with good tools and good
hygiene — the work is the reward. Fun is a legitimate success criterion here, not a
nice-to-have (see below).

### Resolving the built-in tensions

The drivers pull in opposite directions, so the brief settles the rules up front.

**Tension 1 — build vs. adopt (learning vs. control/privacy).**

- **Learning** says "reimplement it — that's the point."
- **Control/privacy** says "reuse libraries freely; what matters is owning the trust
  boundary."

**The rule:** building is licensed for learning, and **control/privacy is the hard
constraint that bounds what may be reused.** Reuse anything that doesn't compromise the
trust boundary (a codec library, a web framework, a container runtime). Do not adopt
anything that phones home, carries telemetry, or puts a third party in the path to your
media or its access control. Every future "build vs. adopt" decision is settled by this
rule rather than re-argued from scratch — and recorded as an ADR when it's load-bearing.

**Tension 2 — AI everywhere vs. control/privacy.** Heavy AI use collides with "no third
party in the path to your media." The trust boundary cleanly bifurcates AI by *what it
operates on*:

- **Build-time AI** operates on source, PRs, and project artifacts — not on the user's
  media. Cloud models are fine here (this coding agent included).
- **Runtime / in-product AI** operates on the user's media stream. It is bound by the
  no-third-party rule: **prefer local / self-hosted models; any cloud-model call is a
  trust-boundary decision requiring an ADR.** This makes in-product AI a *better*
  learning target anyway — running models locally is more instructive than gluing to an
  API.

**Tension 3 — ambition vs. finishing.** A from-scratch media server × control/privacy ×
AI-everywhere × full SDLC rigor is a lot of maxed dimensions for a first home project.
The failure mode of "done properly" is a museum of half-built, impeccably engineered
subsystems, none finished. **The guardrail:** the engineering-craft layer exists to
prevent exactly this — ruthless sequencing, WIP limits, vertical slices over horizontal
perfection. Ship a thin thing end-to-end before deepening any one layer. If the process
ever adds ceremony without adding speed or learning, cut it.

## Who it's for (primary personas)

See [personas](../requirements/personas.md). In short: **me** (operator/builder, the
person learning) and **my household** (people who just want to press play and have it
work, with zero tolerance for config files).

## What success looks like

Concrete, observable outcomes — the user-facing bar and the project's actual reasons for
existing.

**User-facing (it has to actually work):**
- A new movie dropped on the server is watchable in the **main TV room** within minutes,
  with correct metadata and artwork, no manual steps. The main room is a projector + AV
  receiver driven by an **Xbox Series X** (primary playback device), with a **PS4 Pro**
  as backup. A working Xbox client is the minimum bar for "it plays."
- Surround audio reaches the AV receiver intact — multichannel tracks are not silently
  downmixed to stereo when direct play would have preserved them.
- Family members can browse and play without touching a config file.
- I can watch my library from a hotel on my phone, securely, **without exposing the home
  network** — an instance of the control/privacy principle, not a separate goal.

**Learning & craft (why we're building it at all):**
- I can explain, from memory, how every layer works — transcoding, transport, playback,
  remote access — because I built or deliberately chose each one.
- The project demonstrably advanced my AI practice: documented agent skills/tooling that
  made the build faster, and at least one genuine in-product AI feature running locally.
- The SDLC holds up to scrutiny: a real roadmap, traceable epics/features/stories, and a
  reproducible build → test → package → release path.

**Control / privacy (verifiable, not assumed):**
- Nothing in the running product phones home or routes my media or its access control
  through a third party. The build-time/runtime AI split is honoured and auditable.
- Every load-bearing build-vs-adopt and AI-placement choice is captured as an ADR.

**Fun (the honest one):**
- I still want to work on it. Sustained momentum is itself a signal the approach is right.

## Explicit non-goals

What we are deliberately NOT building (at least not now). This bounds scope as much as
the goals do:

- **Competing with Jellyfin/Plex on feature breadth or time-to-value.** We will be
  slower and narrower on purpose.
- **Shipping this to other people / other households.** Single operator, single home.
  No multi-tenant, no hosting-as-a-service.
- **AI features for their own sake.** In-product AI must earn its place as a real,
  used feature — not a tech demo bolted on to justify the word "AI".
- Live TV / DVR / tuner integration.
- Music-library parity with dedicated audio servers.
- Photo management.
- Replacing a NAS / general file management.

## Guiding principles / constraints

- **Self-hosted only.** Runs on commodity home hardware I own.
- **The console is the first-class client.** The primary playback target is a 10-foot,
  game-controller-driven experience on the Xbox Series X (PS4 Pro as backup), not a
  desktop browser. This shapes client architecture and UI navigation from day one; a
  laptop/phone browser is a secondary convenience, not the design centre.
- **Own the trust boundary.** No phone-home, no telemetry, no third party in the path to
  the media or its access control. Security-first for anything internet-facing.
  Build-time AI is cloud-OK; runtime AI prefers local models (see tensions above).
- **Learning value is a first-class reason.** "We could just adopt X" is not an automatic
  win; building X may be correct when the understanding is the point — bounded only by
  the control/privacy rule.
- **Done properly, to go faster.** Engineering hygiene and a planned roadmap are the
  enabling structure for AI-assisted speed, not bureaucracy. Process that doesn't earn
  its keep gets cut. Detailed conventions live in
  [ways of working](../process/ways-of-working.md), not here.
- **Finish thin slices first.** Vertical end-to-end over horizontal perfection; limit
  work-in-progress.
- **Open formats** where it costs nothing to prefer them.
- **Decisions are recorded.** Load-bearing choices become ADRs; the brief is the thing
  they trace back to.
