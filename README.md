# Best Ball Knowledge — public data mirror

This repo is the public-data sidecar for the [Best Ball Knowledge](https://github.com/dhanani1/best-ball-knowledge)
Chrome extension. It hosts the JSON files the extension fetches at runtime so daily
ADP / rank refreshes and contest / schedule / trade updates reach end users
**without requiring an extension reinstall**.

## Files

| File | What it is | Updated |
|--|--|--|
| `players.json` | Per-player ADP, FPTS consensus rank, projections, and settling-model state for the active draft window | Daily by the maintainer's cron |
| `contests.json` | Underdog contest metadata: entry counts, bracket round structure, payout outline, ID surfaces used by the extension's contest matcher | When new Underdog contests appear |
| `schedule_2026.json` | 2026 NFL weekly matchup + bye-week reference. Used by the recommender's bracket / bring-back / lookahead scoring | Rare (in-season corrections only) |
| `player_team_overrides.json` | Manual team patches for trades the Underdog slate metadata hasn't caught up to yet | When trades happen |

All four files are in the **FPTS-flavor** that the public Best Ball Knowledge
extension distribution consumes. The R&D-flavor (ETR + LegUp consensus) is
NOT mirrored here — R&D is a single-user development build.

## What's NOT here

The extension's scoring weights (`contest_weights.json`, `baseline_weights.json`,
`build_budgets.json`) and correlation profiles (`correlations.json`) are the output
of fitting work in the private code repo. They ship bundled with the extension
zip and only change at refit cadence (weeks). Updating those requires a normal
extension version bump.

## How the extension reads from here

The extension's `data_loader.js` resolves each file in this order:

1. **Cache** — `chrome.storage.local` keyed entry, 4-hour TTL. If fresh, return without touching the network.
2. **Remote** — `GET https://raw.githubusercontent.com/dhanani1/best-ball-knowledge-data/main/<file>` with `cache: 'no-store'` and a 10s timeout. On `200`, write to cache and return.
3. **Stale cache** — if remote fails but a cached copy (any age) exists, return it.
4. **Bundled fallback** — fetch the copy that shipped with the extension zip.
5. **None** — return `{ source: 'none', data: null }`.

The sidebar footer reads `Data: <Nh ago>` when the source is `'remote'` or
`'cache'`, and switches to `Data: bundled fallback · check network` on `'bundled'`.
Hover the footer for a per-file source breakdown.

Concurrent loads for the same file are deduped via an in-flight Promise map so
multi-init paths (sidebar boot, adapter boot, exposures boot) don't race-fetch.
Cache entries carry a `schema_version` so forward-incompatible data-shape changes
can be invalidated by bumping `CACHE_SCHEMA_VERSION` in `data_loader.js`.

## How updates land here

The maintainer's daily cron (`scripts/fetch_daily.py` in the private code repo)
runs at ~8am CT. After it pushes the FPTS distribution snapshot to the private
code repo's `origin/main` and the R&D variant to the `rd` remote, a final step:

1. Shallow-clones this public repo into a temp dir via SSH.
2. Copies the four files above from the freshly-built local data folder.
3. Commits with message `daily(public-data): refresh @ YYYY-MM-DD` if there's a diff.
4. Pushes to `main`.
5. Cleans up the temp dir.

Fail-silent — if this step errors, the private code-repo pushes already
succeeded, so the bundled fallback for users continues to work. The next cron
run picks up the diff.

## License

Data is sourced from publicly available rank aggregators (FantasyPoints) and
Underdog Fantasy's own slate metadata. No original research output is published
here.
