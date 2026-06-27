# Functional Requirements

> **DRAFT skeleton.** Grouped by capability area. Each requirement gets a stable ID.
> Status: `proposed` → `accepted` → `implemented`.

## Capability areas (the domain, first cut)

- **LIB** — Library management (scanning, organisation, collections)
- **META** — Metadata & artwork enrichment (identification, scraping, caching)
- **STREAM** — Streaming & playback session management
- **XCODE** — Transcoding (format/bitrate adaptation)
- **CLIENT** — Client applications (TV, mobile, web, casting)
- **AUTH** — Identity, authentication, authorisation
- **REMOTE** — Remote access & connectivity
- **ADMIN** — Server administration, config, observability

## Example entries (to be replaced with real ones)

| ID | Requirement | Persona | Priority | Status |
|----|-------------|---------|----------|--------|
| FR-LIB-001 | The system shall detect new media files added to a watched location without manual intervention. | operator | P0 | proposed |
| FR-META-001 | The system shall automatically identify a media item and fetch title, summary, and artwork. | operator, home-user | P0 | proposed |
| FR-STREAM-001 | The system shall resume playback from the last position per user per item. | home-user | P1 | proposed |
| FR-REMOTE-001 | The system shall allow authenticated playback from outside the home network without exposing internal services directly. | remote-user | P2 | proposed |
