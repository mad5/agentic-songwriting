---
name: song-story
description: >-
  Develops the narrative STORY.md for a Musicay song: characters, conflict,
  arc, and what must land in lyrics vs stay subtext. Use when song-compose
  dispatches the story role or when refining song narrative before lyrics.
---

# Musicay · Song Story

Write the story the song tells — not the finished lyric lines.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · song-story — START

| Field | Value |
| --- | --- |
| Focus | Narrative arc and lyric-facing beats |
| Context | {song_root} · round {NNN} |
```

## Inputs

- `BRIEF.md` (vision, language, constraints; `## Album context` when present)
- Existing `STORY.md` if present
- Round ≥ 2: `ANALYSIS.md`, editor review
- Album tracks: album `STORY.md` / `SEQUENCE.md` beat for this index (read-only)

## Outputs

- `STORY.md` (full rewrite or focused revision)
- Write prose in the BRIEF Language for narrative content aimed at later lyrics

## Workflow

1. Derive logline from emotional core + Original Input.
2. Define characters/POV (even if “I” only).
3. Map Opening → Escalation → Turning point → Resolution/open end to song sections later.
4. List **must land in lyrics** vs **subtext**.
5. Round ≥ 2: fix story holes called out by editor/ANALYSIS; do not invent a new unrelated plot.

## Quality bar

- One clear conflict
- Arc fits a song length (not a novel)
- Lyric-facing beats are concrete images/actions

**Final response:**

```
## MUSICAY · song-story — COMPLETE

| Field | Value |
| --- | --- |
| Story | {song_root}/STORY.md |
| Logline | {one sentence} |
| Next | song-structure |
```
