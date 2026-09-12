# state.json schema (album)

Path: `albums/<slug>/state.json`

```json
{
  "slug": "example-album",
  "phase": "interviewing",
  "round": 0,
  "status": "running",
  "language": "en",
  "theme": "short theme summary",
  "track_count": 0,
  "interview_step": 1,
  "tracks": [],
  "last_commit": null,
  "critic_sessions": 0,
  "created_at": "2026-09-12T00:00:00Z",
  "updated_at": "2026-09-12T00:00:00Z"
}
```

## Fields

| Field | Type | Description |
| --- | --- | --- |
| `slug` | string | Directory name under `albums/` |
| `phase` | string | See phase values |
| `round` | integer ≥ 0 | Album composing round (`0` during interview/planning) |
| `status` | string | Operational status (mirrors song where useful) |
| `language` | string | ISO-ish code; **default `en`** |
| `theme` | string | Short theme from user input |
| `track_count` | integer ≥ 0 | Planned / actual track count |
| `interview_step` | integer \| `"done"` | Current interview checklist step |
| `tracks` | array | Track index entries (see below) |
| `last_commit` | string \| null | Short git hash after last commit |
| `critic_sessions` | integer ≥ 0 | Count of `/album critic` sessions |
| `created_at` | string | ISO-8601 UTC |
| `updated_at` | string | ISO-8601 UTC |

## Phase values

| Value | Meaning |
| --- | --- |
| `interviewing` | Collecting structural decisions |
| `planning` | Album roles writing BRIEF/STORY/SEQUENCE/STYLE-BIBLE |
| `composing` | Track song rounds in progress |
| `awaiting_user` | Gate open |
| `paused` | User paused |
| `done` | FINAL.md written |
| `blocked` | Failed mid-run |

## Status values

| Value | Meaning |
| --- | --- |
| `running` | Work in progress |
| `awaiting_user` | Gate open (may duplicate phase) |
| `paused` | User paused |
| `done` | Finished |
| `blocked` | Failed |

## Track entry

```json
{
  "index": 0,
  "slug": "last-train-home",
  "title": "Last Train Home",
  "path": "tracks/00-last-train-home",
  "role": "opener",
  "song_round": 1,
  "status": "awaiting_user",
  "recommendation": "weiter"
}
```

| Field | Type | Description |
| --- | --- | --- |
| `index` | integer ≥ 0 | Track order |
| `slug` | string | Title slug without numeric prefix |
| `title` | string | Working / final title |
| `path` | string | Relative path under album root |
| `role` | string | opener, lead_single, single, deep_cut, ballad, closer, interlude, … |
| `song_round` | integer | Current song round in that folder |
| `status` | string | Song-folder status |
| `recommendation` | string \| null | From last track ANALYSIS |

Version `state.json` in git (do not gitignore).
