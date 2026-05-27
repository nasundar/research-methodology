# Case study — reviewer disjointness: the verdict-swap event

> **Source.** Cold-tier paper rev-4, cycle v8 in the seed session.
> Specifics redacted; the pattern is preserved.

> **Purpose.** Document the single cleanest empirical demonstration in
> the seed session that two reviewers from disjoint model families
> catch largely disjoint bug classes. Single-reviewer protocols would
> have graduated a paper with a load-bearing contradiction.

---

## Setup

Standing reviewer pair: a Claude variant (Reviewer A) and a GPT variant
(Reviewer B). Both running the same close-review prompt, both reading
the same revision file, parallel dispatch. The pair had been operating
for 19 cycles across two papers by this point in the session.

The paper in question (the cold-tier companion paper) was at rev-4,
its 8th close-review cycle. Reviewer B had been holding the paper at
MAJOR REVISIONS for **5 consecutive cycles** (v3, v4, v5, v6, v7) on
a single load-bearing critique: a specific axis was load-bearing in a
way the closed-form was missing.

Rev-4 was the revision that folded the experiment that closed
Reviewer B's standing thread (see `case-studies/productive-
falsification-case.md` case 2).

## The verdict-swap

When the rev-4 reviews came back:

| Reviewer | Prior verdict (v7) | Verdict on rev-4 (v8) | Direction |
|---|---|---|---|
| A (Claude variant) | MINOR | **MAJOR ESCALATED** | ↓ worse |
| B (GPT variant) | MAJOR (5 cycles) | **MINOR** | ↑ better |

**Same paper, same revision, opposite verdict trajectories.**

### Why Reviewer B improved

The new experiment closed B's 5-cycle thread. The substantive content
landed correctly. B's specific load-bearing critique had been
addressed; B had no remaining basis for MAJOR.

### Why Reviewer A escalated

The same revision that closed B's thread introduced **three new
arithmetic bugs**:

- A specific minimum-threshold value was reported as `N_min = 50K
  events` in 6 paper sections. The paper's own derivation gives
  171K; the experiment's RESULTS § 6.3 says 147K. The 50K number
  was **fabricated** — neither value in the paper's own evidence
  base produced 50K.
- A corresponding "3× reduction" claim, derived from the fabricated
  N_min, propagated through 5 sections.
- A closure-claim count ("all 7 prior failures closed") became
  wrong under the corrected number.

Reviewer A's strength is internal-arithmetic verification. A checked
the numbers, found the fabrication, and escalated.

## What this means

### Either reviewer alone is insufficient

- **Had the team used only Reviewer A:** the paper would have stayed
  at MINOR-or-better for 5 consecutive cycles on the missing-
  experiment thread, *because A never thought the missing experiment
  was load-bearing*. The paper would have graduated without ever
  closing B's thread, and the load-bearing axis would have been
  missing from the published version.
- **Had the team used only Reviewer B:** the paper would have
  graduated at rev-4, *because B's thread was closed and B did not
  catch the N_min fabrication*. The paper would have shipped with a
  fabricated value propagating across 6 sections — a
  paper-retraction-class bug.

Neither single-reviewer outcome would have been the right outcome.
The pair caught both classes of bug in the same cycle, on the same
revision.

### The disjointness pattern

This is one data point in a larger pattern across the 19-cycle
session:

| Reviewer A (Claude variant) caught | Reviewer B (GPT variant) caught |
|---|---|
| Internal-arithmetic correctness | Arithmetic execution under load |
| Overclaim detection | Cross-section drift |
| Closure-count audits | Convention violations |
| "you claimed X; reality is Y" rebuttals | "your number doesn't match your derivation" |

The disjointness is not perfect. At at least one cycle (v9), both
reviewers caught the same one-line leftover. But the pattern is clear
enough that the two reviewers can be characterised as **specialising
on different bug classes** — and the specialisations are
**complementary**, not redundant.

## What the pair does NOT solve

Both reviewers are LLM-based and share many priors. Neither caught a
specific `±25%` vs `±30%` band attribution drift until both
surfaced it in the same cycle (under the same prompt — neither was
faster than the other). This suggests:

- LLM reviewers share blind spots correlated with model family.
- Cross-family rotation (Claude × GPT) gives orthogonal coverage on
  **execution-class** bugs.
- Cross-modality rotation (LLM × programmatic linter × human) would
  give orthogonal coverage on **systematic** bugs — but the seed
  session did not validate this extension.

## How to set this up for yourself

For each paper revision:

1. Pick two reviewers from **disjoint model families** (Claude × GPT;
   GPT × Gemini; LLM × human; etc.). Same model family pairs share
   priors and the disjointness collapses.
2. Both reviewers run the **same prompt** in parallel.
3. Both reviewers' outputs are cached side-by-side (e.g., as
   `experiments/00NN-cluster-review/data/<paper>-<rev>-<reviewer>.md`).
4. The orchestrator (the agent talking to the user) reads both reports
   side-by-side and reconciles.

## How to read two reviewer reports side-by-side

For each finding:

- **In both reports:** weight strongly. Almost certainly real.
- **In one report only:** read both reviewers' framings of the
  surrounding context. Often the silent reviewer didn't weight the
  issue as load-bearing.
- **Verdict mismatch (one ACCEPT, one MAJOR):** the MAJOR is almost
  certainly right. ACCEPT-without-MAJOR-cross-check is fragile.

When verdicts disagree on severity, default to MAJOR. The asymmetry
of cost favours fixing the lower-confidence issue (one cycle delay)
over shipping a load-bearing contradiction (potential retraction).

## Operational cost

In the seed session aggregate (19 cycles × 2 reviewers), the pair
cost was approximately 12% of total session compute. This was
considered proportionate to the value: every L1.21 cycle that
escaped the V1–V6 protocol was caught by **exactly one** of the two
reviewers.

For tighter budgets: use the pair on the **final 2–3 cycles** of a
graduation, and a single reviewer on earlier cycles. The early cycles
tolerate single-reviewer noise; the final cycles do not.

## The verdict-swap as a signal

The verdict-swap pattern itself — one reviewer's verdict improving
while the other's regresses, on the same revision — is a strong
**signal**:

- It typically means the revision **closed a major thread**
  (reviewer who was MAJOR is now MINOR).
- AND the revision **introduced new content that has new bugs**
  (reviewer who was clean is now flagging A1-class).

This is the L1.21 anti-pattern (playbook 07) at its most visible.
When you see a verdict-swap, the response is:

1. Apply V1–V6 (playbook 06) to close the new bugs.
2. Do **not** ship the revision until both reviewers return without
   verdict-regression in the next cycle.
3. Cache the swap as a learning event: which V-step would have
   caught the new bugs? Extend the protocol if necessary.

## What this case did NOT show

The case demonstrates that two disjoint reviewers catch disjoint
bug classes, but it does **not** show:

- That the same two specific reviewers would always be disjoint
  (the disjointness might be regime-dependent).
- That any cross-family pair is equally good (we only tested one).
- That adding a third reviewer would be free of redundancy (it
  might or might not).

These are the open questions the seed session flagged for cross-team
replication. The pattern is empirically robust within the seed
session; whether it transfers to other agentic-research systems is
unverified.

---

*Source: `data-pilot-research/paper/methodology-workshop-draft.md`
§ 5.2 ("The 'verdict swap' event (cold-tier v8)"). Specifics
paraphrased and domain stripped.*
