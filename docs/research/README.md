# Research / Spikes

Time-boxed investigations that inform decisions but aren't decisions themselves.
Each spike: a question, what we did, what we found, and a recommendation that usually
feeds an ADR.

Likely early topics:

- **Streaming protocols** — HLS vs DASH vs progressive; direct-play capability negotiation.
- **Transcoding** — FFmpeg integration patterns; hardware acceleration (QSV/NVENC/VAAPI).
- **Metadata providers** — TMDB/TVDB/MusicBrainz; licensing, rate limits, matching quality.
- **Remote access** — reverse tunnels, relays, VPN (e.g. WireGuard/Tailscale), NAT traversal,
  and their security tradeoffs.
- **Client landscape** — what TV/mobile/cast platforms demand (codecs, DRM, app stores).
- **Prior art** — how Jellyfin/Plex/Emby solve these; what to borrow, what to avoid.

Naming: `NNNN-topic.md`.
