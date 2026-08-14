# lightplayer-stories

Visual story baselines for [LightPlayer Studio](https://github.com/PhotomancerArt/lightplayer),
captured by its `validate-stories` CI job — one **snapshot per lightplayer commit**.

This repo is pure storage: nothing builds here. Studio stories are captured in
lightplayer's pinned CI environment and pushed here as snapshot commits.

## Layout

Each snapshot commit contains:

- `images/` — the full story PNG set (one file per story × viewport).
- `manifest.json` — provenance: source commit, CI run URL, pinned tool
  versions, and which files the comparison judged `stale` vs `tolerated`.

Snapshot commits are **parented on the snapshot they were compared against**,
so GitHub's compare view shows exactly the changed stories with
swipe/onion-skin: `https://github.com/PhotomancerArt/lightplayer-stories/compare/<base>...<head>`.

## Refs

| Ref | Meaning |
|---|---|
| `sha-<lightplayer-sha>` | Snapshot of that lightplayer main commit (acceptance baseline) |
| `pr-<number>` | Latest capture for that lightplayer PR (force-updated per run) |
| `latest` | Always the newest main snapshot (GC-exempt; lightplayer's README hero images point here) |

Baselines for a PR come from the nearest captured ancestor of its merge-base
with main; **merge-to-main is acceptance** — there is no baseline commit step.

## Retention

A scheduled workflow deletes old refs: `pr-*` after 30 days, `sha-*` after
180 days (always keeping the newest 50). `latest` and `main` are never
deleted. Unreachable objects are reclaimed by GitHub's background gc, so
old raw-URL images may keep resolving for a while after their ref is gone.

See the ADR in the lightplayer repo (`docs/adr/`) for the full decision record.
