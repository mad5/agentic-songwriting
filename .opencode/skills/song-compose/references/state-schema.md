# state.json schema

Path: `{song_root}/state.json` — typically `songs/<slug>/state.json`, or `albums/<album>/tracks/NN-<slug>/state.json` for album tracks.

```json
{
  "slug": "example-slug",
  "round": 1,
  "status": "running",
  "language": "en",
  "theme": "short theme summary",
  "last_commit": null,
  "critic_sessions": 0,
  "created_at": "2026-09-06T00:00:00Z",
  "updated_at": "2026-09-06T00:00:00Z"
}
```

## Fields

| Field | Type | Description |
| --- | --- | --- |
| `slug` | string | Song directory leaf name (without `NN-` prefix for album tracks, or full folder name — keep consistent with folder) |
| `round` | integer ≥ 1 | Current round number |
| `status` | string | See status values |
| `language` | string | ISO-ish code from BRIEF (**default `en`**) |
| `theme` | string | Short theme from user input |
| `last_commit` | string \| null | Short git hash after last round commit |
| `created_at` | string | ISO-8601 UTC |
| `updated_at` | string | ISO-8601 UTC |

## Status values

| Value | Meaning |
| --- | --- |
| `running` | Round in progress |
| `awaiting_user` | Round done; gate open |
| `paused` | User paused |
| `done` | FINAL.md written; no more rounds |
| `blocked` | Failed mid-round |

Optional (informational, not required):

| Field | Type | Description |
| --- | --- | --- |
| `critic_sessions` | integer ≥ 0 | Count of `/song critic` sessions on current round |
| `album` | string | Album slug when this song root is under `albums/` |
| `track_index` | integer ≥ 0 | Zero-based track order within the album |

Version `state.json` in git (do not gitignore).
