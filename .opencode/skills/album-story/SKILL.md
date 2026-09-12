---
name: album-story
description: >-
  Develops album STORY.md: concept chapters or cohesive collection, motifs, POV,
  and per-track story beats. Use when album orchestrator dispatches the story role
  or when refining album narrative before track lyrics.
---

# Musicay · Album Story

Write the album narrative frame — not finished lyric lines.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · album-story — START

| Field | Value |
| --- | --- |
| Focus | Album narrative mode and per-track beats |
| Context | albums/{slug} · planning |
```

## Inputs

- `BRIEF.md`, `INTERVIEW.md`
- Existing `STORY.md`
- Later rounds: album ANALYSIS, sequence changes

## Outputs

- Complete `STORY.md`: mode (concept vs collection), POV, through-line, motifs, must-land vs subtext, per-track beats table matching planned count

## Workflow

1. Honor interview mode: chapters for concept albums; shared color + through-line for collections.
2. Define motifs/callbacks lightly — enough for lyricists, not a novel.
3. Draft one beat/purpose row per planned track (working titles OK).
4. Later rounds: fix contradictions with SEQUENCE roles and track analyses.

## Quality bar

- Clear mode choice
- Each track has a distinct beat (no clone purposes)
- Subtext list keeps lyrics from over-explaining

**Final response:**

```
## MUSICAY · album-story — COMPLETE

| Field | Value |
| --- | --- |
| Story | albums/{slug}/STORY.md |
| Mode | {concept \| collection} |
| Next | album-sequence + album-style |
```
