# Requirements Conventions (detail)

Reference for [SKILL.md](../SKILL.md). Read this before writing or editing files under
`docs/requirements/`. It encodes the conventions **already in use** in those files — the
goal is consistency, not novelty.

## Contents

- [Identifiers](#identifiers)
- [EARS phrasing](#ears-phrasing)
- [Functional vs non-functional](#functional-vs-non-functional)
- [Traceability](#traceability)
- [Lifecycle and priority](#lifecycle-and-priority)
- [Document templates](#document-templates)

## Identifiers

Stable, permanent, never reused. The whole point is that an issue, ADR, test, or commit can
say "FR-LIB-003" years later and still mean the same thing.

**Functional:** `FR-<AREA>-<NNN>` — three-digit, zero-padded, sequential within the area.

| Area | Scope |
|------|-------|
| `LIB` | Library management — scanning, organisation, collections |
| `META` | Metadata & artwork — identification, scraping, caching |
| `STREAM` | Streaming & playback session management |
| `XCODE` | Transcoding — format/bitrate adaptation |
| `CLIENT` | Client applications — TV, mobile, web, casting |
| `AUTH` | Identity, authentication, authorisation |
| `REMOTE` | Remote access & connectivity |
| `ADMIN` | Server administration, config, observability |

**Non-functional:** `NFR-<CAT>-<NNN>`.

| Category | Scope |
|----------|-------|
| `PERF` | Performance & capacity |
| `MEDIA` | Media compatibility (containers, codecs, client profiles) |
| `SEC` | Security & privacy |
| `OPS` | Reliability & operability |
| `CON` | Constraints (hardware, deployment model) |

**Rules:**

- Allocate the next free number in the area; never renumber existing requirements to close
  gaps. Deletion retires the ID — it is not recycled.
- If a requirement is superseded, mark it superseded and reference the replacement ID;
  don't silently overwrite a live ID with different meaning.
- A new capability area or NFR category is a real decision — add it to `functional.md` /
  `non-functional.md` deliberately, not ad hoc mid-requirement.

## EARS phrasing

EARS (Easy Approach to Requirements Syntax) gives every requirement a clear trigger and a
testable response, which kills the ambiguity that freeform prose smuggles in. Use the
pattern that fits; always use "the system shall".

**Ubiquitous** — always-on, no trigger:

> The system shall `<response>`.
>
> *FR-SEC-… The system shall encrypt all media traffic that leaves the local network.*

**Event-driven** — triggered by an event ("When"):

> When `<trigger>`, the system shall `<response>`.
>
> *FR-LIB-001 When a new media file appears in a watched location, the system shall add it
> to the library without manual intervention.*

**State-driven** — active throughout a state ("While"):

> While `<state>`, the system shall `<response>`.
>
> *FR-STREAM-… While a playback session is active, the system shall record the playback
> position per user per item.*

**Unwanted behaviour** — handling errors/edge cases ("If/Then"):

> If `<unwanted condition>`, then the system shall `<response>`.
>
> *FR-XCODE-… If a client cannot direct-play the source format, then the system shall
> transcode it to a profile the client supports.*

**Optional feature** — only when a capability is present ("Where"):

> Where `<feature is present>`, the system shall `<response>`.
>
> *FR-CLIENT-… Where the client device supports surround audio, the system shall pass
> multichannel tracks through without downmixing to stereo.*

Patterns combine when warranted (e.g. "While streaming remotely, if bandwidth drops below
the current bitrate, then the system shall …"). Keep combinations readable; if it needs
three clauses to parse, it's probably two requirements.

## Functional vs non-functional

- **Functional** = a behaviour you can trigger and observe. Belongs in `functional.md`.
- **Non-functional** = a quality or constraint on behaviour — how *well*, how *fast*, how
  *securely*, under what *limits*. Belongs in `non-functional.md`.

The decisive question: *is this a behaviour, or a constraint on a behaviour?* "Resume
playback from the last position" is functional. "Time-to-first-frame under 2s for direct
play" is non-functional (PERF). "Remote access must not expose internal services to the
internet" is non-functional (SEC) — and a direct expression of the brief's control/privacy
driver, so it's load-bearing.

NFRs must be **measurable**. "Secure", "fast", "reliable" are aspirations until they carry a
number or a testable condition. Pin the reference hardware before writing PERF numbers —
they're meaningless without it.

## Traceability

Every requirement answers "who is this for, and why does it exist?":

- **Persona(s)** — `operator`, `home-user`, `remote-user` (see
  [personas](../../../docs/requirements/personas.md)). A requirement serving no persona is
  suspect.
- **Brief driver** — which north-star reason it advances: the learning vehicle, control/
  privacy, or a specific user-facing success criterion. If you can't name one, it probably
  shouldn't exist.
- **Forward links** (added as they appear) — the use case(s) it realises (`UC-NN`), any ADR
  that decides its mechanism, and the GitHub issue it graduates into once stable.

This is bidirectional insurance: from a requirement you can reach its justification; from a
persona or brief driver you can check it's actually served.

## Lifecycle and priority

- **Status:** `proposed` → `accepted` → `implemented`. New requirements start `proposed`;
  promote to `accepted` once the operator commits to it; `implemented` when code satisfies it.
- **Priority:** reuse the board's `P0`–`P3`. Priority is about *order of work*, not
  importance-in-the-abstract — don't mark everything P0.
- **Persona and priority are different axes.** Persona = who it's for
  (`operator`/`home-user`/`remote-user`); priority = when we do it (P0–P3). Persona slugs
  are words precisely so they can't be misread as a priority.

## Document templates

Keep `functional.md` grouped by capability area, `non-functional.md` grouped by category.
Tabular form suits functional requirements (scannable, sortable); NFRs often need a line of
context, so prose-with-ID or a table both work — match what the file already does.

**Functional entry (table row per requirement, under an area heading):**

```markdown
## LIB — Library management

| ID | Requirement | Persona | Priority | Status |
|----|-------------|---------|----------|--------|
| FR-LIB-001 | When a new media file appears in a watched location, the system shall add it to the library without manual intervention. | operator | P1 | proposed |
```

**Non-functional entry (under a category heading):**

```markdown
## Security & privacy (NFR-SEC)

- **NFR-SEC-001** — The system shall not expose internal services directly to the internet;
  all remote access is mediated. *(remote-user; control/privacy driver.)*
```

When a requirement's rationale or measurement needs more than a line, add a short note
beneath it rather than bloating the table cell. Keep prose hand-wrapped to ~90 cols to match
the rest of the repo, and remember lychee runs offline — any cross-repo link must be an
absolute URL.
