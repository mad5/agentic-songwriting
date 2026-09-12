---
name: album-sequence
description: >-
  Designs album SEQUENCE.md and singles plan: track count, order, energy curve,
  opener/lead-single/closer roles. Use when album orchestrator dispatches the
  sequence role or when reshaping track order before composing.
---

# Musicay · Album Sequence

Own listening order, energy curve, and singles slots. No lyrics.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · album-sequence — START

| Field | Value |
| --- | --- |
| Focus | Track order, energy curve, singles |
| Context | albums/{slug} · planning |
```

## Inputs

- `BRIEF.md`, `STORY.md`, `INTERVIEW.md`
- `COMMERCIAL.md` (update singles sketch)
- Rubric: `.cursor/skills/album/references/commercial-rubric.md`

## Outputs

- Complete `SEQUENCE.md` (count, runtime, energy curve, ordered table with roles)
- Update `COMMERCIAL.md` singles & release sketch to match
- Working titles + roles so orchestrator can write `TRACKLIST.md`

## Workflow

1. Lock track count from interview (default 10–12; EP 5–7 if chosen).
2. Place opener, **lead single often ~track 2**, mid-album depth/ballad, closer.
3. Assign roles: `opener`, `lead_single`, `single`, `deep_cut`, `ballad`, `closer`, optional `interlude`.
4. Flag sequencing risks (sag, three ballads in a row, weak closer).
5. Later rounds: reorder using album-editor / ANALYSIS flags; keep folder rename notes for orchestrator.

## Quality bar

- Distinct single candidates (not three ballads)
- Energy curve readable in one paragraph
- Every row has role + energy note

**Final response:**

```
## MUSICAY · album-sequence — COMPLETE

| Field | Value |
| --- | --- |
| Sequence | albums/{slug}/SEQUENCE.md |
| Track count | {N} |
| Lead single | #{index} |
| Next | TRACKLIST + composing gate |
```
