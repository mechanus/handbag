# Non-Functional Requirements

> **DRAFT skeleton.** The qualities that constrain architecture. These often matter
> more than features for a media server (it lives or dies on playback smoothness and
> remote-access security). Make them measurable.

## Performance & capacity (NFR-PERF)

- Concurrent streams target: _N direct-play + M transcodes on target hardware (define hw)._
- Time-to-first-frame: _< X seconds for direct play._
- Library scale: _up to N items without UI degradation._

## Media compatibility (NFR-MEDIA)

- Source containers/codecs to support: _e.g. MKV/MP4, H.264/H.265/AV1, AAC/AC3/DTS..._
- Client target profiles: _what each client can direct-play vs needs transcoded._

## Security (NFR-SEC)

- Remote access must not expose internal services directly to the internet.
- Authentication model; secrets handling; transport encryption everywhere off-LAN.
- Threat model lives in `docs/architecture` once drafted.

## Reliability & operability (NFR-OPS)

- Restart/recovery behaviour; backup of metadata DB; upgrade strategy.
- Observability: logs, metrics, health.

## Constraints (NFR-CON)

- Self-hosted on commodity home hardware (define the reference machine).
- Deployment model: _containers? single binary? (see architecture / ADRs)._
