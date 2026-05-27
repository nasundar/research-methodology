# Playbook 03 — The Discussions Log

> The discussion brain (playbook 01) lives in a single file:
> `DISCUSSIONS.md`. This playbook describes its conventions, what belongs
> there vs. what belongs in the research log, and how discussions get
> consolidated.

## Purpose

`DISCUSSIONS.md` captures the **human ↔ agent Q&A** about the research:
questions, interpretations, design debates, surprise reactions,
"what does this mean?" exchanges. Anything that is conversational rather
than executable belongs here.

It exists so that interpretations and arguments do not pollute the
citable research record. The research record (milestone log, learnings
ledger, paper drafts) is for things that **happened** (experiments,
revisions, decisions). The discussions log is for things that were
**thought about**.

## Where it lives

A single file `DISCUSSIONS.md` at the root of the research repo. When it
crosses ~30K words, roll over to `DISCUSSIONS-YYYY-qN.md`. Append-only
across all volumes.

## Entry conventions

Each discussion gets its own dated section. The convention:

```markdown
## YYYY-MM-DD — Discussion N: <topic title>

**User question (paraphrased)**: <the user's question or prompt, in your
own words if the original was long>

### Agent response

<the substantive response — design proposal, interpretation,
analysis, debate position>

### Caveats / things to be careful about

<failure modes, limits of the proposal, what could go wrong>

---

*[End of Discussion N]*
```

## Two example entries

### Example 1 — design discussion

```markdown
## YYYY-MM-DD — Discussion 5: Should we add a sketch-based cold tier?

**User question (paraphrased)**: We're at the storage floor on the hot
tier; do we get more leverage by adding a sketch-based cold tier or by
pushing harder on hot-tier compression?

### Agent response

Sketch-based cold tier, because [reasoning…]. The hot-tier compression
gap is bounded below by the entropy of the workload's irreducible
identifier columns; we're already inside 1.2× of that floor.
A sketch tier opens a new operating point (10–100× storage reduction
at declared error) that the hot tier cannot reach by definition.

Suggested next step: dispatch an experiment that prototypes the
cold-tier sketch budget under three error-tolerance bands.

### Caveats

(a) "Cold tier" is a deceptively heavy term — it commits us to a
   second physical store + a query-time tier-routing decision.
(b) We have no real-data validation of the cold-tier mechanism yet;
   the synthetic-only finding is a ceiling.

---

*[End of Discussion 5]*
```

### Example 2 — interpretation / "what does this mean?"

```markdown
## YYYY-MM-DD — Discussion 11: Why is EXP-0030's residual non-monotone?

**User question (paraphrased)**: Look at the EXP-0030 cycle-2 chart.
The residual is non-monotone in the cardinality axis. Is that real?

### Agent response

It looks real but I think it's a denominator artifact. The mix axis
is defined on emitted events, but the writer amplifies one of the
pillars before write, so the realised mix differs from the intended
mix by ~10×. The non-monotonicity may collapse once we re-plot on
realised-mix instead of intended-mix.

Proposed action: not a research-brain mutation yet. Let's verify the
denominator with a cycle-0 audit (playbook 11) before consolidating.

### Caveats

(a) If the cycle-0 audit confirms the artifact, we should backfill a
   rule into AGENT protocol to require knob-realisation audits
   before any sweep.
(b) If the audit refutes the artifact, the non-monotonicity is real
   and worth a mechanism investigation.

---

*[End of Discussion 11]*
```

(Note: this example is loosely based on a real seed-session pattern; the
abstracted version appears in `case-studies/`. The point here is the
*shape* of the entry.)

## What does NOT belong in `DISCUSSIONS.md`

- **Experiment results.** Those go in `experiments/NNNN-*/RESULTS.md`
  and are summarised in `MILESTONE_LOG.md`.
- **Code or formulas that will be committed.** Discussions are prose.
  When prose graduates to artifacts, it migrates through the
  consolidation procedure (playbook 01).
- **Reviewer findings.** Those have their own home in the close-review
  cache directory and are linked from the milestone log.

## What about discussions that produce decisions?

When a discussion produces an actionable decision (e.g., "we'll add a
cold tier; dispatch EXP-NNNN"), the action lives in the research brain
(an `EXP-NNNN` directory + `ITERATIONS.md` line + milestone log entry).
The discussion stays put. The new artifact references the discussion
entry by date and discussion number.

## Sample file header

```markdown
# DISCUSSIONS — `<project-name>`

> **Append-only.** New discussion entries at the bottom. Each discussion
> is a dated, numbered, topical exchange. The research brain
> (`MILESTONE_LOG.md`, `notes/learnings.md`, `paper/*`) is read-only
> during discussions; explicit consolidation per playbook 01 § 3 moves
> material from here to there.
```

## Rollover convention

When the live volume crosses ~30K words:

1. Close the current file with `*[End of volume N — see DISCUSSIONS-...
   for continuation]*`.
2. Create the next volume with its own header and discussion counter
   continuing from the last.
3. Both files stay in the repo forever.

## Reading discipline

When picking up a project after a break:

1. Read the tail of `DISCUSSIONS.md` (last 3–5 discussions) — this is
   the live thinking.
2. Read the tail of `MILESTONE_LOG.md` — this is what has happened.
3. The two together give the "where are we" picture.

---

*Origin: `data-pilot-research/DISCUSSIONS.md` (Discussion 3 made the
model explicit; the file's overall structure is the source of the
convention).*
