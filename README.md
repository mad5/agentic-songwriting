# Agentic Songwriting

Agentic **pre-studio songwriting**: style and lyrics for single songs and full albums. Audio is produced elsewhere (Suno, Udio, a DAW). This repo only decides **how it should sound** and **what it should say**.

Example of the output: 

Album: **Romeo & Juliet**
https://suno.com/playlist/f0b45796-ec39-4274-99f5-03f6f9758b31

Album: **The Long Way to Someday**
https://suno.com/playlist/45222775-4255-43d7-9653-cddb19bd02a6


# TLDR;

```bash
git clone https://github.com/mad5/agentic-songwriting.git
cd agentic-songwriting
opencode
```
**Prompt:**

`/album Two young lovers from feuding families in Verona secretly marry, but a tragic chain of misunderstandings leads to their untimely deaths, which finally ends the bitter rivalry between their households. `



# Long version

Skills follow the [Agent Skills](https://agentskills.io) layout: one folder per skill, each with a `SKILL.md` plus optional `assets/` and `references/`. They ship in [`.cursor/skills/`](.cursor/skills/). Install **the whole set** — orchestrators dispatch the other roles by name.

Default language for written artifacts is **English (`en`)**. Override with `Language: de` (or another code) in the prompt. Chat with the agent can be in any language; files follow the BRIEF `Language` field.

---

## Table of contents

1. [Install](#install)
   - [Cursor](#cursor)
   - [Claude Code](#claude-code)
   - [OpenCode](#opencode)
   - [Verify](#verify)
2. [Song family](#song-family)
3. [Album family](#album-family)
4. [Language and scope](#language-and-scope)

---

## Install

You need an agent that loads filesystem skills (Cursor, Claude Code, or OpenCode), **git** (orchestrators commit after each round), and a clone of this repository. Song and album drafts are written under `songs/` and `albums/` in the project you open — clone this repo and work **inside it**, or copy the skill folders into another git repo.

```bash
git clone https://github.com/mad5/agentic-songwriting.git
cd agentic-songwriting
```

Each skill is a directory whose name matches the `name` in `SKILL.md`:

| Family | Skills |
| --- | --- |
| Song | `song-compose`, `song-director`, `song-story`, `song-structure`, `song-style`, `song-lyricist`, `song-rhyme-critic`, `song-editor`, `song-critic` |
| Album | `album`, `album-director`, `album-story`, `album-sequence`, `album-style`, `album-editor` |

Do not install only the orchestrators. Templates live under `song-compose/assets/` and `album/assets/`; role skills must sit **next to** them so relative paths and sibling lookups work.

### Cursor

Cursor loads project skills from `.cursor/skills/` automatically. After you clone and open this folder as a workspace, the Musicay skills are already in the right place.

1. Open the cloned repo in Cursor (`File → Open Folder`).
2. Use **Agent** chat (not Ask-only).
3. Type `/` and confirm you see `song-compose`, `album`, and the other Musicay skills.
4. Optional: **Customize → Skills** in the sidebar lists discovered skills.

**Global install** (every Cursor workspace on this machine): copy or symlink the skill folders into `~/.cursor/skills/`:

```bash
mkdir -p ~/.cursor/skills
for d in .cursor/skills/*; do
  ln -sfn "$(pwd)/$d" ~/.cursor/skills/"$(basename "$d")"
done
```

On Windows (PowerShell, from the repo root):

```powershell
New-Item -ItemType Directory -Force -Path "$HOME\.cursor\skills" | Out-Null
Get-ChildItem .cursor\skills -Directory | ForEach-Object {
  New-Item -ItemType SymbolicLink -Force `
    -Path "$HOME\.cursor\skills\$($_.Name)" `
    -Target $_.FullName | Out-Null
}
```

Restart Cursor (or start a new Agent chat) if a skill does not appear. Cursor also reads `.claude/skills/` and `.agents/skills/` for compatibility; you do not need those if `.cursor/skills/` is populated.

Docs: [Cursor Agent Skills](https://cursor.com/docs/skills).

### Claude Code

Claude Code does **not** read `.cursor/skills/`. It loads:

| Scope | Path |
| --- | --- |
| This project | `.claude/skills/<skill-name>/SKILL.md` |
| All projects | `~/.claude/skills/<skill-name>/SKILL.md` |

From the repo root, symlink the shipped skills into `.claude/skills/` (commit the `.claude/skills` directory of symlinks only if your team wants that; otherwise keep it local):

```bash
mkdir -p .claude/skills
for d in .cursor/skills/*; do
  ln -sfn "$(pwd)/$d" .claude/skills/"$(basename "$d")"
done
```

Copy instead of symlink if you prefer a self-contained tree:

```bash
mkdir -p .claude/skills
cp -R .cursor/skills/. .claude/skills/
```

Then start Claude Code **in this directory**:

```bash
claude
```

Invoke `/song-compose` or `/album` (the folder name is the slash command). If Claude was already running before the folders appeared, restart it once.

**Global:** same loop, destination `~/.claude/skills/`. Global install applies everywhere, but drafts still need a git working tree — open this repo (or another repo you intend to write `songs/` into).

Docs: [Claude Code skills](https://code.claude.com/docs/en/skills).

### OpenCode

OpenCode does **not** read `.cursor/skills/`. It loads:

| Scope | Path |
| --- | --- |
| This project | `.opencode/skills/<skill-name>/SKILL.md` |
| All projects | `~/.config/opencode/skills/<skill-name>/SKILL.md` |
| Claude-compatible | `.claude/skills/` and `~/.claude/skills/` |
| Agent-compatible | `.agents/skills/` and `~/.agents/skills/` |

If you already created `.claude/skills/` for Claude Code, OpenCode will pick those up. Otherwise:

```bash
mkdir -p .opencode/skills
for d in .cursor/skills/*; do
  ln -sfn "$(pwd)/$d" .opencode/skills/"$(basename "$d")"
done
```

Quit and reopen OpenCode after installing. Ask it to list installed skills and confirm `song-compose` and `album` are present. The agent loads a skill with its `skill` tool (or you type `/song-compose` / `/album` where slash commands are enabled).

Docs: [OpenCode Agent Skills](https://opencode.ai/docs/skills/).

### Verify

In Agent chat:

```text
/song-compose
```

or

```text
/album
```

If the skill is loaded, the agent should follow the Musicay orchestrator (it announces a **MUSICAY · song-compose** or **MUSICAY · album** start table). If nothing happens, the skill folders are not on a path your tool scans — check the tables above and that each folder contains `SKILL.md` directly (not nested one extra directory).

---

## Song family

Use this family for **one song**. The orchestrator `song-compose` bootstraps `songs/<slug>/`, runs specialist roles, writes a round analysis, **commits to git**, then **stops for your decision**. It never auto-loops into the next round.

### What each skill does

| Skill | Role | Writes |
| --- | --- | --- |
| `song-compose` | Orchestrator | `state.json`, `ANALYSIS.md`, `FINAL.md`, git commit, user gate. Does **not** author BRIEF/STORY/STRUCTURE/STYLE/LYRICS itself. |
| `song-director` | Director | `BRIEF.md` — vision, emotional core, audience, tone, constraints, language. |
| `song-story` | Story | `STORY.md` — characters, conflict, arc, what must appear in lyrics vs stay subtext. |
| `song-structure` | Form | `STRUCTURE.md` — sections, hook placement, energy curve. |
| `song-style` | Sound | `STYLE.md` — genre, mood, tempo feel, instrumentation, vocal character, prompt sketch for an external audio tool. |
| `song-lyricist` | Lyrics | `LYRICS.md` — verses, chorus, bridge. |
| `song-rhyme-critic` | Meter (review only) | `rounds/round-NNN/reviews/rhyme-critic.md` — rhyme, stress, singability. Does not edit lyrics. |
| `song-editor` | Dramaturg (review only) | `rounds/round-NNN/reviews/editor.md` — story–lyric coherence, weak lines, concrete alternatives. Does not edit lyrics. |
| `song-critic` | Your ears | Captures test-listener / test-reader notes, then re-runs rhyme critic + editor. Does not increment the round. |

One round order: director → story → structure → **style + lyricist in parallel** → **rhyme critic + editor in parallel**.

### How to run it

In Agent chat (natural language is fine; slash names match the orchestrator skill):

```text
/song <theme or story>
/song continue <slug>
/song status <slug>
/song critic <slug> <your feedback>
```

Equivalent explicit invokes: `/song-compose` with the same arguments, or `/song-critic` for listener notes only.

| Command | What happens |
| --- | --- |
| `/song <theme>` | Derives a hyphenated `slug`, creates `songs/<slug>/` from templates, fills original input, runs **round 1**, writes analysis, commits, asks you what to do next. Optional: `Language: de`. |
| `/song continue <slug>` | Starts the next round only after you chose “another round”. Refuses if status is `done`. |
| `/song status <slug>` | Title, round, status, last recommendation. No role dispatch, no commit. |
| `/song critic <slug> …` | You are the test listener/reader. Feedback is stored; critics analyze. Lyrics are **not** rewritten until `/song continue`. |

**After every successful round** you get a gate. Pick one:

1. **Another round** — then invoke `/song continue <slug>` (or confirm in chat; the next round must be a **new** turn).
2. **Done** — writes `FINAL.md` (style + lyrics + short story/structure summary + export hint) and sets status `done`.
3. **Pause** — status `paused`, no further work.

Do not expect the agent to start round 2 in the same turn as round 1.

### Song artifacts

```text
songs/<slug>/
├── BRIEF.md
├── STORY.md
├── STRUCTURE.md
├── STYLE.md
├── LYRICS.md
├── state.json
├── rounds/round-NNN/
│   ├── reviews/
│   │   ├── rhyme-critic.md
│   │   ├── editor.md
│   │   └── user-critic.md    # only after /song critic
│   └── ANALYSIS.md
└── FINAL.md                  # when you choose Done
```

Typical flow: start with a theme → read `LYRICS.md` and `STYLE.md` → optionally `/song critic` with “this chorus is hard to sing” → `/song continue` to apply changes → repeat until `FINAL.md` → paste the style prompt and lyrics into your audio tool.

---

## Album family

Use this family for a **multi-song project** (concept album or cohesive collection). The orchestrator `album` interviews you for commercial structure, dispatches album-level roles, then **reuses the song family** under each numbered track. It does not copy tracks into `songs/`.

### What each skill does

| Skill | Role | Writes |
| --- | --- | --- |
| `album` | Orchestrator | Interview log, track index, album analysis, git commit, gates. Does **not** author album BRIEF/STORY/SEQUENCE/STYLE-BIBLE/COMMERCIAL bodies itself (except bootstrap/index). Does **not** write track lyrics. |
| `album-director` | Album director | Album `BRIEF.md` — vision, audience, constraints. |
| `album-story` | Album narrative | `STORY.md` — concept vs collection, motifs, POV, per-track story beats. |
| `album-sequence` | Sequence | `SEQUENCE.md` — track count, order, energy curve, opener / lead single / closer. |
| `album-style` | Shared sound | `STYLE-BIBLE.md` — common palette and allowed per-track variation. |
| `album-editor` | Cross-track dramaturg (review only) | Album round reviews — cohesion, singles, skip risk. Does not edit lyrics. |

Track folders then run `song-director` … `song-editor` with the track path as song root, reading album `SEQUENCE.md`, `STORY.md`, and `STYLE-BIBLE.md` as context.

### How to run it

```text
/album <theme>
/album continue <slug>
/album continue <slug> --track NN
/album status <slug>
/album critic <slug> <your feedback>
```

Optional: `Language: de|en|…` on the first `/album` call.

| Phase | What you do | What the agent does |
| --- | --- | --- |
| **Interview** | Answer **one** structural question per turn (title, concept vs collection, track count, singles, energy curve, …). You can skip remaining questions explicitly. | Appends Q&A to `INTERVIEW.md`. Recommends a default each time. |
| **Planning** | Starts automatically after the interview checklist is complete. | Dispatches director → story → **sequence + style bible in parallel**, then updates `TRACKLIST.md` / commercial framing. Commits. |
| **Plan gate** | Choose **start songs**, **adjust plan**, or **pause**. | Does **not** compose tracks in the same turn as planning unless you chose start songs. |
| **Composing** | `/album continue <slug>` after “start songs”. | Creates `tracks/00-<song-slug>/` … and runs **song round 1 sequentially** per track, then album-editor + album analysis, commit, album gate. |
| **Album gate** | Another round, done, or pause. | Further rounds re-run song rounds for tracks still marked `weiter` (or all). `--track NN` limits work to one track. |
| **Critic** | `/album critic <slug> …` (optional `track 03` in the text). | Listener notes at album level; album-editor (and song critics if tracks are named). Does not bump the album round. |

### Album artifacts

```text
albums/<slug>/
├── BRIEF.md
├── STORY.md
├── SEQUENCE.md
├── STYLE-BIBLE.md
├── COMMERCIAL.md
├── TRACKLIST.md
├── INTERVIEW.md
├── state.json
├── rounds/round-NNN/
│   ├── reviews/
│   └── ANALYSIS.md
├── FINAL.md
└── tracks/
    ├── 00-<song-slug>/     # full song artifact tree + own rounds
    ├── 01-<song-slug>/
    └── …
```

Typical flow: `/album` a theme → finish the interview → review sequence and style bible at the plan gate → start songs → listen through tracks → `/album critic` or another album round → `FINAL.md` when the set holds together.

---

## Language and scope

- **Artifacts** (briefs, stories, lyrics, style docs): BRIEF `Language` (`en` unless you set another code).
- **Chat:** any language you prefer.
- **Out of scope:** generating audio, MIDI, or DAW sessions; automatic multi-round loops without a user gate; treating this as a production studio.

When a song is finished, use `FINAL.md` (and album `STYLE-BIBLE.md` / track `STYLE.md`) as the brief for whatever tool actually renders sound.
