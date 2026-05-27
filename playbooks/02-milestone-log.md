# Playbook 02 — The Milestone Log

> An append-only chronological "what was done" record of the research
> project. One file (`MILESTONE_LOG.md`), one writer at a time, never
> rewritten in place.

## Purpose

The milestone log is the **front-to-back read** of the research project.
A reader six months from now should be able to start at line 1 and end at
the most recent entry, and follow the entire research arc — every closed
milestone, every paper revision, every reviewer outcome — without
referring to any other file except the experiments themselves.

## Where it lives

A single file `MILESTONE_LOG.md` at the root of the research repo.

## Entry conventions

Each entry is a self-contained section. The convention:

```markdown
## YYYY-MM-DD — <milestone-id or revision-id> — <one-line title>

- **Req:** what the user asked for (paraphrased if needed). 1–3 lines.
- **Res:** what was delivered. Commit SHA + branch tag if applicable.
- **Headline findings:** bullet list. Each finding cites the experiment
  (`experiments/NNNN-<slug>`) that produced it. Quantitative claims must
  carry units and seed counts.
- **Reviewer outcomes:** if a reviewer pair was dispatched at this
  milestone, summarise each verdict (1–2 lines per reviewer) and link to
  the cached close-review files.
- **Decisions:** what got carried forward, what got dropped, what's
  queued for the next milestone. Explicit "advance / re-roll / kill".
- **Artifacts:** files produced — `notes/*.md`, `paper/*.md`, charts,
  code crates.
- **Next steps:** the queue for the next milestone.
```

## Two example entries

### Example 1 — milestone close

```markdown
## YYYY-MM-DD — M2: Lenses + cross-pillar index

- **Req:** Add the per-pillar indexes and the cross-pillar correlation
  layer; validate against real data.
- **Res:** Shipped through commit `abc1234`. Three new candidate layouts
  landed; correlation-index sketch validated.
- **Headline findings:**
   1. **C-B-Index reduces cross-pillar query p99 by 3.4× vs C-A baseline**
      at 10M events (3-seed CI [3.22, 3.54]). EXP-0017.
   2. **Real-data round-trip closes the synthetic-vs-real moments gap**
      to within 2× on logs and traces; metrics still 2.94× off pending
      a real metric corpus. EXP-0018, EXP-0019.
- **Reviewer outcomes:**
   - Reviewer A: MINOR. Caught two narrowing requirements (L8.4 scope
     limit; cold/warm gap clarification). Findings cached at
     `experiments/00NN-mN-cluster-review/data/cluster-review-vN-A.md`.
   - Reviewer B: MINOR-to-MAJOR for venue X; MINOR for venue Y. Caught
     one arithmetic inconsistency in § 5.2.
- **Decisions:** All blockers closed in `m2-fix` bundle commit `def5678`.
  Advance to M3.
- **Artifacts:** `notes/m2-lenses.md`, `paper/draft.md` rev-4, two charts.
- **Next steps:** M3 cardinality stress at 1B+ scale.
```

### Example 2 — fix bundle (sub-milestone)

```markdown
## YYYY-MM-DD — M2-fix — review-fix bundle shipped

- **Commit:** `def5678`.
- **Bug fixes:** narrow L8.4 in `notes/learnings.md` to low-match-count
  regime (per reviewer B blocker #1); correct § 5.2 arithmetic
  (1,000 + 1,000 + 909 = 2,909, not 2,740).
- **Methodology hardening:** add bootstrap CIs to the latency summary;
  pin seeds in `paper/reproducibility.md`.
- **Decision:** advance to M3.
```

## Anti-patterns

- **Do not rewrite past entries.** If an old entry is wrong, append a
  correction entry referencing it. Old entries stay verbatim.
- **Do not pre-write entries.** The log is *closes* — only commit a
  milestone's entry when the milestone is actually closed.
- **Do not interleave with discussion.** Discussion stays in
  `DISCUSSIONS.md`. The milestone log is for research-brain mutations.
- **Do not skip "what we got wrong".** Every milestone has 1–3 things
  that the reviewers flagged or that the team learned. Capture them.
- **Do not skip reviewer outcomes.** Even a "MINOR / accept" verdict
  belongs in the log — it's part of the verdict trajectory (playbook 13).

## Sample header

The first non-empty section of the file should be:

```markdown
# MILESTONE LOG — `<project-name>`

> **Append-only.** New entries at the bottom. Each milestone has: date,
> request, response, headline findings, reviewer outcomes, decisions,
> artifacts, and next steps.
>
> Read this file front-to-back to follow the entire research arc; jump
> to the bottom for the latest state.
```

## How to read the milestone log

When picking up a project after a break:

1. Read the **first** entry (the project's origin and original ask).
2. Skim the headers of intermediate milestones.
3. Read the **last** 2–3 entries in full.
4. From the last entry's "next steps", you know exactly what comes next.

For a deeper catch-up, follow the `experiments/NNNN-*/RESULTS.md` links
referenced in the last 2–3 milestone entries.

---

*Origin: `data-pilot-research/MILESTONE_LOG.md` first ~200 lines and the
seed-session-wide convention. Generalised here to strip
domain-specific examples.*
