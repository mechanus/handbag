# Personas

> **DRAFT.** Refine names, needs, and constraints together. Personas keep us honest
> about who each feature serves.

Each persona has a stable slug (in backticks) used as its identifier in requirement and
use-case traceability — words rather than numbers, to avoid colliding with the `P0`–`P3`
priority scale.

## Operator (`operator`)

- **You.** Runs and maintains the server. Comfortable with config, containers, networks.
- Wants control, observability, and a system that's pleasant to extend.
- Pain tolerance for setup: high. Pain tolerance for daily-use friction: low.

## Household viewer (`home-user`)

- **Family.** Watches on the lounge TV / their own devices. Not technical.
- Wants: turn it on, find something, press play. Zero config, zero jargon.
- Will abandon the system instantly if it's flaky or confusing.

## Travelling viewer (`remote-user`)

- **You, away from home** (hotel, mobile data). Wants secure access to the library.
- Constraints: variable/limited bandwidth, untrusted networks, NAT/firewalls,
  smart-TV/casting in unfamiliar environments.
