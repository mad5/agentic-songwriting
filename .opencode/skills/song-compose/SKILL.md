---
name: song-compose
description: >-
  Orchestrates Musicay songwriting rounds for style and lyrics. Bootstraps
  songs/<slug>/, dispatches role subagents, writes ANALYSIS.md, commits to git,
  and gates the next round on the user. Use when invoking /song, /song continue,
  /song status, /song critic, or when starting/refining a song concept before
  external audio tools.
---

# Musicay · Song Compose

Orchestrate pre-studio songwriting: **style + lyrics only**. No audio production.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · song-compose — START

| Field | Value |
| --- | --- |
| Focus | {one sentence} |
| Context | {song_root} · round {NNN} |
```

<HARD-GATE>
The orchestrator must **NEVER** write or rewrite BRIEF/STORY/STRUCTURE/STYLE/LYRICS content itself (except bootstrap from templates with user input, ANALYSIS.md, FINAL.md, and state.json). Every role step is delegated via the **Task** tool to a sub-agent instructed to read and follow the matching skill under `.cursor/skills/`.
</HARD-GATE>

## Song root

Standalone songs live under `songs/<slug>/`. Album tracks use the same skills under `albums/<album-slug>/tracks/NN-<lied-slug>/`. Always pass the absolute **song root** to sub-agents; they work only under that path.

## Invocation

```text
/song <thema oder geschichte>   — neuer Song, Runde 1
/song continue <slug>           — nächste Runde nach Nutzer-Freigabe
/song status <slug>             — Stand ohne neue Runde
/song critic <slug> <feedback>  — Nutzer als Test-Hörer/Leser; Team-Analyse
```

Optional flags in free text: `Language: de|en|…` (**default `en`** if unspecified).

## Quick start

### New song (`/song …`)

1. Derive `slug` (2–5 words, lowercase, hyphens) from the theme.
2. Create `songs/<slug>/` from [assets/](assets/) templates.
3. Fill `BRIEF.md` Original Input verbatim; set `Language`; leave other brief fields for the director.
4. Write `state.json` (see [references/state-schema.md](references/state-schema.md)): `round: 1`, `status: running`.
5. Create `rounds/round-001/reviews/`.
6. Run **one full round** (dispatch order below).
7. Write `rounds/round-NNN/ANALYSIS.md`, update `state.json`, **git commit**, then **user gate**.

### Continue (`/song continue <slug>`)

1. Load `songs/<slug>/state.json`. Refuse if `status` is `done`.
2. Increment `round`, set `status: running`, create `rounds/round-NNN/reviews/`.
3. Run full round → ANALYSIS → commit → gate.

### Status (`/song status <slug>`)

Summarize BRIEF title, round, status, and last ANALYSIS recommendation. Do not dispatch roles or commit.

### Critic pass (`/song critic <slug> <feedback>`)

Load `.cursor/skills/song-critic/SKILL.md` and follow it. Summary:

1. Parse verbatim feedback (everything after slug).
2. Append session to `rounds/round-NNN/reviews/user-critic.md` — **do not** increment `round`.
3. Dispatch **parallel**: `song-rhyme-critic` + `song-editor` (must read latest user-critic session).
4. Write critic-focused `ANALYSIS.md` (user feedback mapping first).
5. Commit `song(<slug>): critic pass round NNN — …`, gate.

Refuse if `status: done`. Allowed from `awaiting_user`, `paused`, or `running` (recover blocked first).

## Round dispatch order

Dispatch each step with **Task** (`subagent_type: generalPurpose`). Instruct the sub-agent to read `.cursor/skills/<skill>/SKILL.md` and work only under the given **song root** (absolute path).

| Step | Skill | Mode |
| --- | --- | --- |
| 1 | `song-director` | sequential |
| 2 | `song-story` | sequential |
| 3 | `song-structure` | sequential |
| 4 | `song-style` + `song-lyricist` | **parallel** |
| 5 | `song-rhyme-critic` + `song-editor` | **parallel** (reviews only) |

Pass absolute song root, round number (`NNN` zero-padded), and language from BRIEF/state. When the song root is an album track, also pass album path so roles can read SEQUENCE / STORY / STYLE-BIBLE (read-only).

On sub-agent failure: set `status: blocked`, report, stop (no commit unless partial artifacts should be saved — prefer commit with message noting failure).

## ANALYSIS.md

Write `{song_root}/rounds/round-NNN/ANALYSIS.md` after all role steps. Include:

- Stärken / Schwächen (Lyrics, Reime, Story, Stil, Struktur)
- Offene Punkte (priorisiert)
- Empfehlung: `weiter` | `fast fertig` | `fertig`

Use critic review scores and production artifacts; do not invent praises that contradict reviews.

**Critic pass** (`/song critic`): use [assets/ANALYSIS-CRITIC-TEMPLATE.md](assets/ANALYSIS-CRITIC-TEMPLATE.md). User feedback mapping is section 1; recommendation defaults to `weiter` when user reported substantive issues.

## Git commit (end of every successful round)

Stage `songs/<slug>/` only (or whole repo on first bootstrap of skills — not needed mid-song).

```bash
git add songs/<slug>/
git commit -m "$(cat <<'EOF'
song(<slug>): round NNN — <kurze Zusammenfassung>

EOF
)"
```

Record `last_commit` (short hash) in `state.json`. Never amend; never `--no-verify` unless user asks.

## User gate (mandatory)

After commit, set `status: awaiting_user` and emit GATE status. Ask the user (AskQuestion when available) with exactly three options:

1. **Weitere Runde** — user will invoke `/song continue <slug>` (or confirm in chat → then continue in a **new** turn after confirmation)
2. **Fertig** — write `FINAL.md` (assemble STYLE + LYRICS + brief story/structure summary + export hint for external tool), set `status: done`, commit `song(<slug>): finalize`
3. **Pausieren** — set `status: paused`, no further work

**Never** start the next round in the same turn without an explicit user choice for „Weitere Runde“.

**Final response:** Last element when reaching a phase boundary:

```
## MUSICAY · song-compose — GATE

| Field | Value |
| --- | --- |
| Song | {song_root} |
| Round | {NNN} |
| Recommendation | {weiter \| fast fertig \| fertig} |
| Commit | {short hash} |
| Next | Weitere Runde · Fertig · Pausieren |
```

Other statuses: `COMPLETE` (status-only), `BLOCKED`, `FAILED`, `AWAITING` (artifacts written, waiting).

## Language

All role skills must write narrative/lyrics in the BRIEF `Language` code (**default `en`**). Orchestrator passes it in every Task prompt.

## Out of scope

Audio, MIDI, DAW, automatic multi-round loops, Coby work-items.
