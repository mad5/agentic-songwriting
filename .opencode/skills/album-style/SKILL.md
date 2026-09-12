---
name: album-style
description: >-
  Writes STYLE-BIBLE.md shared sonic palette and allowed per-role variation for
  an album. Use when album orchestrator dispatches the style role or when
  refining how a Musicay album should sound across tracks.
---

# Musicay · Album Style

Describe how the **album** should sound for external generators. No audio files.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · album-style — START

| Field | Value |
| --- | --- |
| Focus | Shared palette and allowed variation |
| Context | albums/{slug} · planning |
```

## Inputs

- `BRIEF.md`, `STORY.md`, `SEQUENCE.md` if present
- `INTERVIEW.md` (genre, comps, variation)
- Existing `STYLE-BIBLE.md`

## Outputs

- Complete `STYLE-BIBLE.md` including album-level prompt sketch and per-role variation table

## Workflow

1. Match genre/mood to emotional thesis and comps.
2. Define tempo feel **range** and signature sounds (recognition).
3. Specify allowed shifts per role so tracks are not clones.
4. List forbids (e.g. sudden genre break unless SEQUENCE outlier).
5. Later rounds: resolve style clashes called out in album ANALYSIS.

## Quality bar

- Specific enough to guide every track STYLE.md
- Variation rules prevent skip-from-sameness
- Prompt sketch self-contained

**Final response:**

```
## MUSICAY · album-style — COMPLETE

| Field | Value |
| --- | --- |
| Style bible | albums/{slug}/STYLE-BIBLE.md |
| Genre | {short} |
| Next | TRACKLIST + composing gate |
```
