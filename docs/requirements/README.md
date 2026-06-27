# Requirements

How we capture requirements:

- **Personas** ([personas.md](personas.md)) — who we're building for.
- **Use cases** ([use-cases.md](use-cases.md)) — concrete scenarios, in user terms.
- **Functional requirements** ([functional.md](functional.md)) — what the system does,
  grouped by capability area.
- **Non-functional requirements** ([non-functional.md](non-functional.md)) — qualities:
  performance, security, reliability, operability.

Each requirement gets a stable ID (e.g. `FR-LIB-001`, `NFR-SEC-003`) so issues,
ADRs, and tests can reference it. Requirements graduate into GitHub Issues (epics →
features) once they're stable enough to plan against.
