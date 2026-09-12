---
name: song-style
description: >-
  Writes STYLE.md genre, mood, tempo feel, instrumentation ideas, vocal
  character, and an external-tool prompt sketch. Use when song-compose
  dispatches the style producer or when refining how a Musicay song should sound.
---

# Musicay · Song Style

Describe how the song should **sound** for an external generator. No audio files.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · song-style — START

| Field | Value |
| --- | --- |
| Focus | Sonic identity and exportable style prompt |
| Context | {song_root} · round {NNN} |
```

## Inputs

- `BRIEF.md` (tone, audience, constraints)
- `STORY.md`, `STRUCTURE.md`
- Existing `STYLE.md`; round ≥ 2 ANALYSIS
- Album tracks: album `STYLE-BIBLE.md` — stay inside palette unless SEQUENCE allows outlier

## Outputs

- `STYLE.md` complete, including **Prompt sketch for external tool** (style only; do not paste full lyrics there unless the tool workflow needs a one-line theme)

## Workflow

1. Match genre/mood to Brief emotional core and story setting.
2. Describe tempo feel and instrumentation as evocative lists.
3. Define vocal character (intimate, belting, spoken-sung, choir stacks, etc.).
4. Write a paste-ready prompt paragraph for external tools.
5. Round ≥ 2: resolve style/lyric tone mismatches from ANALYSIS.

## Quality bar

- Specific enough to guide generation; not a DAW session plan
- Consistent with Brief tone
- Prompt sketch is self-contained

**Final response:**

```
## MUSICAY · song-style — COMPLETE

| Field | Value |
| --- | --- |
| Style | {song_root}/STYLE.md |
| Genre | {short} |
| Next | critics after lyricist |
```
