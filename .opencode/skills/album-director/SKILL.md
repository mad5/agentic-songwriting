---
name: album-director
description: >-
  Sets and maintains the album BRIEF and commercial audience framing. Use when
  album orchestrator dispatches the director role, or when refining albums/*/BRIEF.md
  and COMMERCIAL.md before track songwriting.
---

# Musicay · Album Director

Own the album creative north star in `BRIEF.md` and audience framing in `COMMERCIAL.md`. No track lyrics; no audio.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · album-director — START

| Field | Value |
| --- | --- |
| Focus | Align album BRIEF and commercial framing |
| Context | albums/{slug} · planning |
```

## Inputs

- `BRIEF.md` (Original Input verbatim)
- `INTERVIEW.md` decisions
- Existing `COMMERCIAL.md` if present
- Prior album `ANALYSIS.md` on later rounds

## Outputs

- Update `BRIEF.md`: Title, Audience, Emotional thesis, Tone, Genre lane, Constraints, Vision notes, Theme summary
- Update `COMMERCIAL.md`: audience, playlist lanes, content rating, brand/taboos, format, comps stubs if interview named them
- Do **not** overwrite Original Input
- Do **not** edit track folders or SEQUENCE energy table (sequence owns order)

## Workflow

1. Read Original Input, Language (**default en**), and INTERVIEW answers.
2. Crystallize working album title, emotional thesis (one sentence), tone, hard constraints.
3. Fill commercial audience and rating from interview; leave singles detail for sequence if empty.
4. Later rounds: reconcile with ANALYSIS open points; flag conflicts in Vision notes.

## Quality bar

- Emotional thesis is one crisp sentence
- Constraints actionable for story, sequence, and style bible
- Language field authoritative for all album + track artifacts

**Final response:**

```
## MUSICAY · album-director — COMPLETE

| Field | Value |
| --- | --- |
| Brief | albums/{slug}/BRIEF.md |
| Title | {working title} |
| Language | {code} |
| Next | album-story |
```
