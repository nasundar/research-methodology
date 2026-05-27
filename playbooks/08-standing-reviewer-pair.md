# Playbook 08 — The Standing Reviewer Pair

> Every review is run by **two reviewers from disjoint model families**,
> in parallel, using the same prompt. The two verdicts are then read
> side-by-side by the orchestrator. Single-reviewer protocols are
> insufficient and produce graduate papers with load-bearing
> contradictions.

## The setup

Pick two large-language-model reviewers from *different model families*.
The seed session standardised on `claude-opus-4.7-xhigh` +
`gpt-5.5`. Other valid pairings: a Claude variant + a GPT variant; a
GPT variant + a Gemini variant; either of the above + a domain-expert
human.

The pair is "standing" — it does not rotate across cycles. The same two
reviewers see every revision so they can track verdict trajectories
(playbook 13) and so the orchestrator can calibrate which reviewer
catches which bug class.

## The dispatch protocol

For each review cycle:

1. Both reviewers receive the **same prompt** (e.g.,
   `prompts/close-review.md`).
2. Both reviewers receive the **same revision file** (e.g.,
   `paper/rev-N.md`).
3. Both reviewers emit:
   - A **verdict**: `ACCEPT` / `MINOR` / `MAJOR` / `REJECT`.
   - A **structured findings list**: A1 (blocking), A2 (substantive
     non-blocking), A3 (cosmetic).
4. The two reviews are cached in the close-review directory
   (`experiments/<NN>-<slug>/data/<rev>-<reviewer>.md`).
5. The orchestrator reads both side-by-side and reconciles.

The dispatch runs in **parallel** (wall-clock cost ≈ 1× single
reviewer; token cost ≈ 2×).

## Why two reviewers, not one

The seed session ran 19 paired review cycles across two papers. The
empirical record is that the two reviewers catch **largely disjoint
bug classes**:

| Reviewer style | Specialises on |
|---|---|
| Reviewer A (Claude-class) | Overclaims, internal arithmetic verification, completeness audits |
| Reviewer B (GPT-class) | Arithmetic execution, cross-section drift, convention violations |

The disjointness is not perfect — sometimes both flag the same bug —
but it is reliable enough that the two reviewers function as
**specialists on different bug classes**.

A single-reviewer protocol would have shipped at least one paper in
the seed session with a load-bearing contradiction (see the verdict-
swap case study).

## The verdict-swap event

The single most striking data point in the seed session was a
verdict-swap on cold-tier rev-4 (cycle v8):

- Reviewer B had been holding the paper at MAJOR for 5 consecutive
  cycles on a specific missing-experiment thread.
- Rev-4 folded the missing experiment, closing reviewer B's thread.
  Reviewer B: **MINOR** (closes 5-cycle thread).
- The same rev-4 introduced three new arithmetic bugs in the
  presentation of the new experiment.
- Reviewer A had been at MINOR. Reviewer A on rev-4: **MAJOR ESCALATED**
  (3 new arithmetic bugs).

**Same paper, same revision, opposite verdict trajectories.** Had the
team used only reviewer B, they would have graduated rev-4 with the
arithmetic bugs. Had they used only reviewer A, they would have
graduated rev-3 without ever closing the 5-cycle thread.

The case is preserved in detail in `case-studies/reviewer-disjointness-case.md`.

## What the pair does NOT solve

The reviewer pair is two LLM-based reviewers. They share many priors.
**Cross-family diversity** (i.e., adding a non-LLM reviewer — a human,
a programmatic linter, a statically-typed numerical re-checker) is the
next axis of orthogonal coverage. The seed session did not validate
this extension. If you have the capacity, add it.

In particular, neither reviewer caught a specific `±25%` vs `±30%`
band attribution drift until both surfaced it in the same cycle. A
mechanical linter that greps for `±N%` patterns and recomputes each
against the source CSV would have caught it earlier.

## How to read two reviewer reports side-by-side

For each finding:

1. **In both reviews:** weight strongly. Almost certainly real.
2. **In one review only:** read both reviewers' framings of the
   surrounding context. Often the silent reviewer didn't think the
   issue was load-bearing, in which case the speaking reviewer's
   weighting matters.
3. **Verdict mismatch (one ACCEPT, one MAJOR):** the reviewer who
   said MAJOR is almost certainly right. ACCEPT-without-MAJOR-cross
   -check is fragile in the seed session's experience.

When verdicts disagree on severity (one MINOR, one MAJOR), default to
**MAJOR**. The asymmetry of cost favours fixing the lower-confidence
bug; the cost of pushing back is a one-cycle delay, the cost of
shipping is a load-bearing contradiction.

## Cost calibration

The reviewer pair costs roughly:

- **2× tokens** of a single-reviewer protocol.
- **1× wall time** (the reviews run in parallel).

In the seed session aggregate (19 cycles × 2 reviewers), the pair cost
was approximately 12% of total session compute. This was considered
proportionate to the value: every L1.21 cycle that escaped the V1–V6
protocol was caught by *exactly one* of the two reviewers.

For projects on tighter budgets: use the pair on the **final 2–3
cycles** of a paper graduation, and a single reviewer on earlier
cycles. The early cycles tolerate single-reviewer noise; the final
cycles do not.

## Rotation discipline

The pair is "standing" within a paper. **Between papers**, rotating
the model variants (e.g., upgrading to a newer model release) is
fine and even healthy — it keeps the pair from over-fitting to a
single failure-mode catalogue.

When rotating mid-paper (rare, but happens), document the rotation in
the milestone log and check the verdict trajectory for a regime
change.

## Anti-patterns

- **Single-reviewer dispatch.** Will graduate a paper with a load-
  bearing contradiction. Documented in the verdict-swap case study.
- **Same-family pair.** Two Claude variants or two GPT variants share
  too many priors. The disjointness on which the pair pattern relies
  collapses.
- **Sequential dispatch instead of parallel.** If reviewer B sees
  reviewer A's report before writing their own, B's review is
  contaminated. Parallel dispatch (and parallel context windows) is
  required.
- **Cherry-picking the friendlier verdict.** "Reviewer A said ACCEPT,
  let's go with that." Reviewer B said MAJOR for a reason; read it.

---

*Origin: `data-pilot-research/paper/methodology-workshop-draft.md` § 5
("The Standing Reviewer Pair Pattern") and §§ 7.1–7.2 (verdict
trajectories).*
