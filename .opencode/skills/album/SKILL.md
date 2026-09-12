---
name: album
description: >-
  Orchestrates Musicay album planning and track songwriting. Bootstraps
  albums/<slug>/, interviews for commercial structure, dispatches album roles,
  then reuses song-* skills for each numbered track. Use when invoking /album,
  /album continue, /album status, /album critic, or planning a multi-song project.
---

# Musicay · Album

Orchestrate pre-studio **album** planning + track lyrics/style. No audio production.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · album — START

| Field | Value |
| --- | --- |
| Focus | {one sentence} |
| Context | albums/{slug} · phase {phase} · round {NNN} |
```

<HARD-GATE>
The orchestrator must **NEVER** write or rewrite album BRIEF/STORY/SEQUENCE/STYLE-BIBLE/COMMERCIAL content itself (except bootstrap from templates with user input, INTERVIEW.md, TRACKLIST.md index updates, ANALYSIS.md, FINAL.md, and state.json). Album role steps go via **Task** to sub-agents following `.cursor/skills/album-*`. Track BRIEF/STORY/STRUCTURE/STYLE/LYRICS go via existing **song-*** skills under the track path — never write track lyrics yourself.
</HARD-GATE>

## Invocation

```text
/album <theme>                    — new album, interview phase
/album continue <slug>            — next phase after user gate
/album continue <slug> --track NN — refine one track only
/album status <slug>              — summary, no writes
/album critic <slug> <feedback>   — test-listener feedback (album-wide)
```

Optional: `Language: de|en|…` (**default `en`** if unspecified).

## Language

Narrative, lyrics, and style prompts use BRIEF `Language` (**default `en`**). Chat with the user may be German; artifacts follow `Language`. Pass language in every Task prompt.

## Quick start

### New album (`/album …`)

1. Derive `slug` (2–5 words, lowercase, hyphens) from the theme.
2. Create `albums/<slug>/` from [assets/](assets/) templates; create empty `tracks/`.
3. Fill `BRIEF.md` Original Input verbatim; set `Language` (default `en`).
4. Write `state.json` (see [references/state-schema.md](references/state-schema.md)): `phase: interviewing`, `round: 0`, `interview_step: 1`.
5. Ask **one** interview question per turn (see [references/interview-guide.md](references/interview-guide.md)); append answers to `INTERVIEW.md`. Recommend a default each time.
6. After the last interview question: set `phase: planning`, dispatch album roles (below), write `TRACKLIST.md`, commit, **plan gate**.

### Continue (`/album continue <slug>`)

1. Load `albums/<slug>/state.json`. Refuse if `phase`/`status` is `done`.
2. Branch on gate choice / phase:
   - After **plan gate → Songs starten**: set `phase: composing`, bootstrap every track folder, run **song round 1 sequentially** for all tracks, then album-editor + album ANALYSIS → commit → album gate.
   - After **album gate → Weitere Runde**: increment album `round`, re-run song rounds for tracks with recommendation `weiter` (or all if unspecified); optional `--track NN` limits to one track.
   - After **Plan anpassen**: resume interview or re-dispatch album roles as needed, then plan gate again.

### Status (`/album status <slug>`)

Summarize title, phase, round, track statuses, last ANALYSIS recommendation. No dispatch, no commit.

### Critic pass (`/album critic <slug> <feedback>`)

1. Parse verbatim feedback after slug (optional track hint like `track 03`).
2. Append session to `rounds/round-NNN/reviews/user-critic.md` — **do not** increment album round.
3. Dispatch `album-editor`; if tracks named, also `song-rhyme-critic` + `song-editor` on those track roots (parallel).
4. Write critic-focused album `ANALYSIS.md`; commit; gate.

## Interview

One question per turn. Use AskQuestion when available. Protocol: [references/interview-guide.md](references/interview-guide.md). Record Q&A in `INTERVIEW.md`. Do not start album role dispatch until the guide checklist is complete (or user explicitly skips remaining questions).

## Album role dispatch (planning)

After interview completes:

| Step | Skill | Mode |
| --- | --- | --- |
| 1 | `album-director` | sequential |
| 2 | `album-story` | sequential |
| 3 | `album-sequence` + `album-style` | **parallel** |
| 4 | Orchestrator | Write/update `TRACKLIST.md` + `COMMERCIAL.md` fields from SEQUENCE (index only; commercial body may be filled by director/sequence as their skills specify — prefer sequence for singles plan, director for audience) |

Pass absolute album path and language. On failure: `phase: blocked`, report, stop.

## Plan gate (mandatory)

After planning commit, set `phase: awaiting_user` and ask:

1. **Songs starten** — `/album continue <slug>` bootstraps tracks and runs song round 1 for all
2. **Plan anpassen** — more interview / re-plan
3. **Pausieren** — `phase: paused`

**Never** start composing all tracks in the same turn as planning without explicit „Songs starten“.

## Track bootstrap + song reuse

For each row in `TRACKLIST.md` / `state.tracks[]`:

1. Create `albums/<slug>/tracks/NN-<lied-slug>/` (`NN` zero-padded).
2. Copy song templates from `.cursor/skills/song-compose/assets/`.
3. Fill track `BRIEF.md`: Language from album; Original Input = album theme + track role + neighbor notes; fill `## Album context`.
4. Track `state.json`: same song schema + `"album": "<slug>"`, `"track_index": N`.
5. Dispatch song roles with **SONG_ROOT** = absolute track path (not `songs/…`):

| Step | Skill | Mode |
| --- | --- | --- |
| 1 | `song-director` | sequential |
| 2 | `song-story` | sequential |
| 3 | `song-structure` | sequential |
| 4 | `song-style` + `song-lyricist` | **parallel** |
| 5 | `song-rhyme-critic` + `song-editor` | **parallel** |

Instruct song sub-agents to read album `SEQUENCE.md`, `STORY.md`, `STYLE-BIBLE.md` (and prior tracks' `LYRICS.md` only for motif continuity — do not copy lines).

Process tracks **sequentially** (00, then 01, …). Write each track's `rounds/round-001/ANALYSIS.md` after its critics.

## Album ANALYSIS (after all track rounds in a composing pass)

Dispatch `album-editor`, then write `albums/<slug>/rounds/round-NNN/ANALYSIS.md`:

- Strengths / weaknesses (arc, singles, variety, skip risk)
- Open points (prioritized)
- Recommendation: `weiter` | `fast fertig` | `fertig`

Use [references/commercial-rubric.md](references/commercial-rubric.md) and [assets/ANALYSIS-TEMPLATE.md](assets/ANALYSIS-TEMPLATE.md).

## Git commit

```bash
git add albums/<slug>/
git commit -m "$(cat <<'EOF'
album(<slug>): <phase or round NNN> — <short summary>

EOF
)"
```

Record `last_commit` in `state.json`. Never amend; never `--no-verify` unless user asks.

## Album gate (after composing pass)

1. **Weitere Runde** — `/album continue <slug>`
2. **Fertig** — write `FINAL.md` from [assets/FINAL-TEMPLATE.md](assets/FINAL-TEMPLATE.md), set `phase: done`, commit `album(<slug>): finalize`
3. **Pausieren** — `phase: paused`

**Final response** at phase boundaries:

```
## MUSICAY · album — GATE

| Field | Value |
| --- | --- |
| Album | albums/{slug} |
| Phase | {phase} |
| Round | {NNN} |
| Recommendation | {weiter \| fast fertig \| fertig \| plan ready} |
| Commit | {short hash} |
| Next | {gate options} |
```

Other statuses: `COMPLETE`, `BLOCKED`, `FAILED`, `AWAITING` (e.g. mid-interview).

## Out of scope

Audio, MIDI, DAW, automatic multi-album loops, Coby work-items, copying album tracks into `songs/`.
