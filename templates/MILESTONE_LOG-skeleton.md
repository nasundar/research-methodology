# MILESTONE LOG — `<project-name>`

> **Append-only.** New entries at the bottom. Each milestone has: date,
> request, response, headline findings, reviewer outcomes, decisions,
> artifacts, and next steps.
>
> Read this file front-to-back to follow the entire research arc; jump
> to the bottom for the latest state.

---

## YYYY-MM-DD — Research track opened

- **Req:** <one-paragraph paraphrase of the user's original ask>.
- **Decision:** new research repo at <path>. Local-only initially.
- **Standing review pair:** <reviewer A> + <reviewer B> (see
  playbook 08).

---

## M0 — Prior-art survey and synthesis (YYYY-MM-DD)

- **Commits:** `<sha>` (scaffold) → `<sha>` (synthesis) → `<sha>`
  (review fixes).
- **Process:**
   - (Brief description — parallel research agents, master synthesis,
     review rounds.)
- **Headline findings:**
   1. <finding 1>
   2. <finding 2>
- **Reviewer outcomes:**
   - Reviewer A: <verdict>. Cached at <path>.
   - Reviewer B: <verdict>. Cached at <path>.
- **Decision:** advance to M1.
- **Artifacts:** `notes/m0-prior-art.md`, ...
- **Next steps:** M1 — <one-line milestone purpose>.

---

## M1 — <milestone-title> (YYYY-MM-DD)

- **Commits:** `<sha>` ... `<sha>`.
- **Headline findings:**
   1. <finding 1, with EXP-NNNN citation>
   2. <finding 2>
- **Reviewer outcomes:**
   - Reviewer A: <verdict + summary>.
   - Reviewer B: <verdict + summary>.
- **Joint priority for M2:**
   1. <highest-priority follow-up>
   2. <next>
- **Decision:** advance / re-roll. (If re-roll, what specifically.)
- **Artifacts:** ...
- **Next steps:** M2 — <one-line>.

---

## Mi-fix — review-fix bundle shipped (YYYY-MM-DD)

(Use this format when a fix-bundle commit closes blockers from the
prior cluster review before opening the next milestone.)

- **Commit:** `<sha>`.
- **Bug fixes:** <list with file paths>.
- **Quantitative corrections:** <list with old → new>.
- **Methodology hardening:** <list>.
- **Decision:** advance to M<i+1>.

---

## Next milestones (queued)

- **M2 — <title>.** <one-line purpose>.
- **M3 — <title>.** <one-line purpose>.
- ...

---

## How to read this repo from scratch (front-to-back)

1. `README.md`
2. `PLAN.md`
3. `MILESTONE_LOG.md` (this file)
4. `notes/m0-prior-art.md`
5. `notes/learnings.md`
6. `experiments/00NN-*/HYPOTHESIS.md` + `RESULTS.md` + `MECHANISM.md`
   in chronological order
7. `paper/*` (latest revision per draft)

---

*This skeleton lives in `templates/MILESTONE_LOG-skeleton.md`.
Convention from playbook 02.*
