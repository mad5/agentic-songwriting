---
name: song-structure
description: >-
  Designs STRUCTURE.md song form, section purposes, hook placement, and energy
  curve for Musicay songs. Use when song-compose dispatches the structure role
  or when reshaping verse/chorus/bridge layout before lyric drafting.
---

# Musicay · Song Structure

Own the song form skeleton. Descriptive only — no audio/MIDI.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · song-structure — START

| Field | Value |
| --- | --- |
| Focus | Song form aligned to story arc |
| Context | {song_root} · round {NNN} |
```

## Inputs

- `BRIEF.md`, `STORY.md`
- Existing `STRUCTURE.md`, `LYRICS.md` (round ≥ 2) for alignment
- ANALYSIS / reviews when refining

## Outputs

- `STRUCTURE.md` with Form line, Sections table, Hook placement, Dynamics curve
- Section labels **must** be usable as `## [Section]` headers in LYRICS.md

## Workflow

1. Map story beats to sections (Verse carries plot; Chorus carries emotional thesis; Bridge shifts angle).
2. Choose a conventional or intentional variant form; keep singable length.
3. Mark where the hook lives and whether it evolves in the final chorus.
4. Round ≥ 2: adjust form if critics/ANALYSIS show missing payoff or bloat; note removals clearly.

## Quality bar

- Every section has a purpose
- Hook is explicit
- Energy curve matches conflict escalation

**Final response:**

```
## MUSICAY · song-structure — COMPLETE

| Field | Value |
| --- | --- |
| Structure | {song_root}/STRUCTURE.md |
| Form | {short form string} |
| Next | song-style · song-lyricist |
```
