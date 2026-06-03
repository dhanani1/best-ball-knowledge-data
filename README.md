# Best Ball Knowledge — public data mirror

This repo is the public-data sidecar for the [Best Ball Knowledge](https://github.com/dhanani1/best-ball-knowledge)
Chrome extension. It hosts the JSON files the extension fetches at runtime so
daily ADP / rank updates and new-contest additions reach users without
requiring a manual extension reinstall.

## Files

| File | What it is | Updated |
|--|--|--|
| `players.json` | Per-player ADP, FPTS consensus rank, projections, and settling-model state | Daily by cron |
| `contests.json` | Underdog contest metadata (entry counts, bracket structure, payouts) | When new contests appear |
| `schedule_2026.json` | 2026 NFL weekly matchup + bye-week reference | Rare (in-season corrections only) |
| `player_team_overrides.json` | Manual team patches for trades the Underdog slate metadata hasn't caught up to yet | When trades happen |

## What's NOT here

The extension's scoring weights (`contest_weights.json`, `baseline_weights.json`,
`build_budgets.json`) and correlation profiles (`correlations.json`) are the
output of fitting work and remain in the private code repo. They're shipped
bundled with the extension and require a normal extension update to change.

## Usage

Fetched directly via `raw.githubusercontent.com`:

```
https://raw.githubusercontent.com/dhanani1/best-ball-knowledge-data/main/players.json
```

The extension's `BBMDataLoader` uses a 4-hour TTL cache and falls back to the
bundled copy in the extension zip if the remote is unreachable.

## License

Data is sourced from publicly available rank aggregators and Underdog Fantasy's
own slate metadata. No data here is original research output.
