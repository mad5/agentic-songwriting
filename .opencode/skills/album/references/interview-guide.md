# Album interview guide

Ask **one question per turn**. Always offer a **recommended default**. Append each Q&A to `INTERVIEW.md`. Language of chat may be German; record answers clearly. Artifact language defaults to **en** unless the user overrides.

Skip remaining questions only if the user explicitly says to skip / use all defaults.

## Checklist (order)

| Step | Topic | Recommended default |
| --- | --- | --- |
| 1 | Working title + language | Title from theme; **Language: en** |
| 2 | Artist persona / POV | Solo narrator, consistent POV across album |
| 3 | Core theme + emotional thesis (one sentence) | Distill from Original Input |
| 4 | Concept album (chapters) vs cohesive collection | Cohesive collection with shared color (unless theme clearly narrative) |
| 5 | Target audience + genre lane | From theme; name 1–2 playlist/mood lanes |
| 6 | Reference albums (comps) | 2–3 real comps in the lane (user may name their own) |
| 7 | Track count + target runtime | **10–12** full songs, ~35–48 min (offer EP 5–7) |
| 8 | Energy curve: opener type, lead-single slot, ballad placement, closer | Strong opener; **lead single ≈ track 2**; one deep mid-album ballad; memorable closer |
| 9 | Singles plan (2–3) | Hook early; streaming-friendly; not three ballads |
| 10 | Variation vs recognition | Same palette; vary tempo feel / density per track role |
| 11 | Leitmotifs / callbacks | Light callbacks if concept; optional motifs if collection |
| 12 | Features, skits/interludes, intro/outro | No skits by default; optional short intro; no filler interludes |
| 13 | Content rating, taboos, brand fit | Clean-enough for playlists unless theme needs edge; list taboos |
| 14 | Album title = title track or standalone name | Prefer strong album title; title track optional |

## After checklist

1. Confirm summary of decisions with the user (short table).
2. Set `interview_step` to done / `phase: planning`.
3. Dispatch album roles (`album-director` → …).

## Interview.md session format

```markdown
## Q{N} — {topic}

**Asked:** …

**Recommended:** …

**Answer:** …
```
