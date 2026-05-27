# Playbook 04 — The Iterations Log

> The append-only experiment register. One file (`ITERATIONS.md`), one
> entry per experiment (`EXP-NNNN`). The compressed one-line story of
> what was tried and what was found.

## Purpose

`ITERATIONS.md` is the **index** of the research project's experiments.
For every experiment ever run, there is one entry. The full write-up
lives in `experiments/NNNN-<slug>/RESULTS.md`; the iterations log holds
the *summary* of what was hypothesised, what was found, and what was
decided.

If the milestone log is "what happened at coarse grain" (playbook 02),
the iterations log is "what happened at experiment grain". The two are
indices into different layers of the same project.

## Where it lives

A single file `ITERATIONS.md` at the root of the research repo.

## Entry conventions

Each `EXP-NNNN` gets one section. The minimum template:

```markdown
## EXP-NNNN — <one-line title>

- **Date:** YYYY-MM-DD
- **Hypothesis (1 line):** <the H you committed in HYPOTHESIS.md>
- **Result:** <the headline outcome — PASS / FAIL / SURPRISE / REFUTED — one paragraph>
- **Reviewer verdict:** <ACCEPT / MINOR / MAJOR / REJECT> or "deferred — bundling with EXP-NNNN+k at next milestone"
- **Decision:** <advance / re-roll / kill / queue follow-up>
- **Files:** <list of new/changed code, notes, paper sections, charts>
```

Extended entries may add:

- **Surprise finding:** anything the result revealed that wasn't
  pre-registered.
- **Mechanism:** one-paragraph "why" summary; full version in
  `experiments/NNNN-*/MECHANISM.md`.
- **Hostile-reviewer questions:** anticipated objections + answers
  (reviewer-bait — see playbook 12).
- **Cross-cycle reconciliation:** if this experiment's result changes
  the interpretation of a prior result, explicitly call out the
  reconciliation.

## Two example entries

### Example 1 — short entry, clean PASS

```markdown
## EXP-0007 — Baseline writer at the canonical scale sweep

- **Date:** YYYY-MM-DD
- **Hypothesis (1 line):** The baseline encoding is scale-invariant
  to within ±5 % across {1M, 10M, 100M} events.
- **Result:** ✅ PASS. Encoded size flat to ±3 % across the sweep.
  Throughput drops ~14 % over 100× (page-cache pressure, not writer-side).
- **Reviewer verdict:** deferred — bundling with EXP-0008/0009 at the
  M1 close-review.
- **Decision:** Advance to EXP-0008 (full query mix).
- **Files:** `experiments/0007-*/RESULTS.md`; one new chart; minor
  edit to `notes/learnings.md` (L3.1).
```

### Example 2 — long entry, refutation that opens a new mechanism

```markdown
## EXP-NNNN — Reviewer-proposed linear term refuted; piecewise discovered

- **Date:** YYYY-MM-DD
- **Hypothesis (1 line):** A reviewer proposed that cost scales linearly
  with cohort count at 50–100 units per cohort.
- **Why this experiment exists:** Five consecutive reviewer cycles
  held the paper at MAJOR over the missing-linear-term critique;
  the experiment is the smallest grid that breaks the coupling and
  tests the term on its own.
- **Result:** ❌ REFUTED with very high confidence. Fit returns a
  *negative* slope and three orders of magnitude off the prior.
  Wrong direction, wrong magnitude. The reviewer's hypothesis is dead.
- **The productive payoff:** With the linear hypothesis killed, the
  cell-by-cell residuals reveal a **piecewise-by-row-group** structure
  at the natural-storage-block boundary. Below threshold: Regime A
  constants. Above threshold: Regime B (inherits the existing closed-
  form + a per-block term).
- **Cross-cycle reconciliation:** A prior OoS soft-fail at −16.86 %
  becomes +3.35 % PASS under the new piecewise model.
- **Reviewer verdict:** standing 5-cycle MAJOR thread closed.
- **Decision:** Advance to EXP-NNNN+1 (validate piecewise at a third
  decade; pin the threshold for the standard writer configuration).
- **Hostile-reviewer questions:** see RESULTS § 7.
- **Files:** `experiments/NNNN-*/`, `notes/learnings.md` (3 new L-IDs),
  `paper/draft.md` § 6 rev.
```

## Anti-patterns

- **Do not skip experiments.** Every measurement, even an exploratory
  smoke test, gets an entry. If it's exploratory, label it so
  (`EXP-NNNNa — smoke test`).
- **Do not pre-write the entry.** The entry lands when the experiment
  closes (either PASS, FAIL, REFUTED, or KILLED).
- **Do not interleave with the milestone log.** The iterations log is
  fine-grained; the milestone log is coarse-grained. Each milestone
  cites the EXPs it folded.
- **Do not let "deferred" verdicts pile up indefinitely.** When you
  defer a verdict to the next milestone close, the milestone close
  must reconcile every deferred entry.
- **Do not silently edit prior entries.** If a prior entry's
  conclusion is invalidated by a later experiment, **append** a
  cross-cycle reconciliation note to the *later* entry; do not
  retroactively rewrite the older one.

## Sample file header

```markdown
# ITERATIONS — `<project-name>`

> **Append-only log of every hypothesis tested.** One block per experiment.
> Format: `EXP-NNNN` | date | hypothesis (1 line) | result | reviewer
> verdict | decision (advance / re-roll / kill).
> Full write-ups live in `experiments/NNNN-<slug>/RESULTS.md`.
```

## How EXP-NNNN numbering works

- Numbering is **sequential** across the project lifetime, not reset
  per milestone.
- The slug is short and descriptive
  (e.g., `0007-scale-sweep`, `0050-real-mix`).
- The directory is `experiments/NNNN-<slug>/` with three required files:
  `HYPOTHESIS.md`, `RESULTS.md`, `MECHANISM.md`. Plus raw data under
  `data/` (gitignored if large) and charts under `charts/`.

## Co-evolution with `notes/learnings.md`

When an experiment produces a durable finding, it lands as an L-ID in
`notes/learnings.md` (the learnings ledger). The iterations log lists
the EXP; the learnings ledger consolidates the L-IDs. The two
references each other:

- ITERATIONS.md `EXP-NNNN` → produces L-IDs `Lx.y, Lz.w`
- learnings.md `Lx.y` → evidence: `experiments/NNNN-*/RESULTS.md`

If two parallel experiments produce overlapping L-IDs, the orchestrator
renumbers the later one and updates inbound citations.

---

*Origin: `data-pilot-research/ITERATIONS.md` first ~200 lines and the
seed-session-wide convention; generalised here to strip
domain-specific examples.*
