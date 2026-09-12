---
name: song-critic
description: >-
  Captures test-listener / test-reader feedback from the user and routes it
  through rhyme critic and editor for team analysis. Use when invoking
  /song critic <slug>, when the user reports how lyrics sound when read or
  sung aloud, or when subjective listening notes should drive change analysis.
---

# Musicay · Song Critic (Nutzer-Feedback)

Der Nutzer ist **Test-Hörer:in** oder **Test-Leser:in**. Subjektive Wahrnehmung („klingt konstruiert“, „unnatürliche Reime“, „Chorus hängt“) ist gültiges Input — kein Bug-Report mit Zeilennummern nötig.

**Announce before start:** First visible element of every response. Emit exactly as H2 + table — no extra blank lines inside the table:

```
## MUSICAY · song-critic — START

| Field | Value |
| --- | --- |
| Focus | User feedback → team analysis |
| Context | {song_root} · round {NNN} |
```

## Invocation

```text
/song critic <slug> <freitext-feedback>
```

Beispiel:

```text
/song critic last-train-home die einzelnen Sätze hören sich nicht nach natürlicher Sprache an, sondern konstruierte reime. das sollte natürlicher klingen.
```

Alles nach `<slug>` ist **verbatim** Nutzer-Feedback (Groß-/Kleinschreibung beibehalten). Song root: `songs/<slug>/` (standalone). Album-Track-Feedback läuft über `/album critic`.

## Orchestration (via song-compose)

song-compose lädt diesen Skill bei `/song critic`. Der Orchestrator:

1. Lädt `{song_root}/state.json`. Verweigern wenn `status: done`.
2. **Keine** Round-Inkrementierung — Feedback bezieht sich auf den aktuellen Stand (`round` in state).
3. Schreibt/ergänzt `rounds/round-NNN/reviews/user-critic.md` (Template unten).
4. Setzt `status: running`.
5. Dispatch **parallel** (Task, `generalPurpose`), jeweils mit Hinweis auf `user-critic.md`:
   - `song-rhyme-critic` — Priorität: Nutzer-Wahrnehmung Reim/Metrik/Natürlichkeit
   - `song-editor` — Priorität: Nutzer-Wahrnehmung Story/Satzrhythmus/Bilder
6. Schreibt `rounds/round-NNN/ANALYSIS.md` (critic-pass-Fokus, Template in song-compose).
7. Git commit, `status: awaiting_user`, **user gate** (wie song-compose).

Subagents **dürfen** `LYRICS.md` nicht ändern — nur Reviews und ANALYSIS.

## user-critic.md

Pfad: `{song_root}/rounds/round-NNN/reviews/user-critic.md`

Neue Session pro `/song critic`-Aufruf (append, nicht überschreiben):

```markdown
# User Critic — Test-Hörer / Test-Leser

## Session {SSS} — {ISO-8601 UTC}

### Raw feedback (verbatim)

{exact user text}

### Agent interpretation

| Theme | Summary |
| --- | --- |
| … | … |

### Implicated lines (best effort)

| Location | Line (excerpt) | Why user might flag it |
| --- | --- | --- |
| [Pre-Chorus] | … | … |
```

Session `{SSS}` = dreistellig, fortlaufend pro Datei (001, 002, …). „Agent interpretation“ und „Implicated lines“ füllt der Orchestrator **nach** den Subagent-Reviews in derselben Session (kurz, keine Vollrewrite-Liste).

## Was Review-Rollen tun müssen

Bei vorhandenem `user-critic.md`:

1. **Zuerst** die neueste Session lesen und explizit darauf antworten.
2. Abschnitt **## User feedback response** in `rhyme-critic.md` bzw. `editor.md` (Template in jeweiligen Skills).
3. Konkrete Zeilen nennen, die zur Nutzer-Wahrnehmung passen — auch wenn der Nutzer keine Zeile zitiert hat.
4. Prioritäten für den nächsten Lyricist-Pass aus Nutzer-Sicht ableiten.

## ANALYSIS (critic pass)

Orchestrator priorisiert in ANALYSIS:

1. **User feedback mapping** — Was meinte der Nutzer? Welche Zeilen?
2. **Team consensus** — Stimmen Rhyme critic und Editor zu? Widersprüche?
3. **Change list** — Priorisierte Fixes für `/song continue`
4. **Recommendation** — `weiter` wenn Nutzer-Feedback substantiell; `fast fertig` wenn nur optional; `fertig` nur wenn Nutzer explizit zufrieden signalisiert

## Git commit

```bash
git add songs/<slug>/
git commit -m "$(cat <<'EOF'
song(<slug>): critic pass round NNN — <kurz: Nutzer-Thema>

EOF
)"
```

## User gate

Identisch zu song-compose: **Weitere Runde** (`/song continue <slug>`) · **Fertig** · **Pausieren**.

**Final response:**

```
## MUSICAY · song-critic — GATE

| Field | Value |
| --- | --- |
| Song | {song_root} |
| Round | {NNN} (unchanged) |
| User session | {SSS} |
| Recommendation | {weiter \| fast fertig \| fertig} |
| Commit | {short hash} |
| Next | Weitere Runde · Fertig · Pausieren |
```

## Out of scope

Lyrics direkt umschreiben, neue Story-Runde, Audio-Produktion.
