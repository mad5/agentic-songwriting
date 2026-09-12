---
name: song-rhyme-critic
description: >-
  Reviews LYRICS.md for rhyme quality, meter, stress, and singability; writes
  scores to rounds/round-NNN/reviews/rhyme-critic.md without editing lyrics.
  Use when song-compose dispatches the rhyme critic or when auditing song meter.
---

# Musicay · Song Rhyme Critic

**Review only.** Never modify `LYRICS.md` or other production files.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · song-rhyme-critic — START

| Field | Value |
| --- | --- |
| Focus | Rhyme, meter, singability review |
| Context | {song_root} · round {NNN} |
```

## Inputs

- `LYRICS.md`, `STRUCTURE.md`, `BRIEF.md` (language)
- Quality rubric: `.cursor/skills/song-compose/references/quality-rubric.md`

## Outputs

Only: `{song_root}/rounds/round-NNN/reviews/rhyme-critic.md`

## Review template

```markdown
# Rhyme & Meter Review — Round NNN

## Overall (1–5)

| Dimension | Score | Note |
| --- | --- | --- |
| Rhyme | | |
| Meter | | |
| Singability | | |

## Per section

| Section | Score | Issues | Suggested fix direction (no full rewrite required) |
| --- | --- | --- | --- |
| … | | | |

## Forced rhymes / clunkers

-

## User feedback response

*(Only when `user-critic.md` exists — latest session.)*

| User theme | Lines affected | Rhyme/meter issue | Fix direction |
| --- | --- | --- | --- |
| … | … | … | … |

## Verdict

`needs work` | `polish` | `strong`
```

## Workflow

1. Check each section against structure labels.
2. Score per rubric; cite example lines.
3. Prefer fix *direction* (syllable target, end-word swap) over rewriting the whole song.
4. Do not paste a replacement full lyric file.

**Final response:**

```
## MUSICAY · song-rhyme-critic — COMPLETE

| Field | Value |
| --- | --- |
| Review | {song_root}/rounds/round-{NNN}/reviews/rhyme-critic.md |
| Verdict | {needs work \| polish \| strong} |
| Next | song-compose ANALYSIS |
```
