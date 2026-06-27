# Use Cases

> **DRAFT.** Concrete scenarios in user terms — the raw material for functional
> requirements. Format: actor, goal, trigger, main flow, what makes it succeed.

## UC-01 — Watch a movie on the lounge TV (home-user)

- **Trigger:** Family member sits down to watch.
- **Flow:** Open app on TV → browse/search library → pick a title → press play →
  it plays at the right quality with resume support.
- **Success:** No buffering stalls, correct subtitles/audio track, remembers position.

## UC-02 — Add new content to the library (operator)

- **Trigger:** New media file lands on the server (download, rip, copy).
- **Flow:** System detects it → identifies it → fetches metadata + artwork →
  it appears in the library, correctly categorised.
- **Success:** Hands-off; correct match; visible within minutes.

## UC-03 — Watch from a hotel (remote-user)

- **Trigger:** Travelling, want to watch from the home library on a phone.
- **Flow:** Authenticate remotely → browse → play, with quality adapting to bandwidth.
- **Success:** Secure (no open home-network exposure), watchable on constrained links.

## UC-04 — Resume across devices (home-user/remote-user)

- Start on the TV, finish on a tablet — position carries over.

_Add more: multiple concurrent streams, transcode vs direct-play, parental scoping,
casting from a phone to a TV, offline download for travel..._
