# Playbook 13 — Paper Graduation: the Verdict-Trajectory Model

> A paper is not "done" because the team thinks so. A paper is done when
> a standing reviewer pair (playbook 08) returns ACCEPT verdicts from
> both reviewers across two consecutive cycles, after the team has
> closed all blocking findings and disclosed all honest-flag weaknesses.

## The model

A paper goes through **N close-review cycles** before graduating. At
each cycle, two reviewers (playbook 08) emit a verdict from:

| Verdict | Meaning |
|---|---|
| `REJECT` | Foundational problems; substantial rework before re-review. |
| `MAJOR REVISIONS` | Blocking findings; substantive rewrite needed; expect another cycle. |
| `MINOR REVISIONS` | Substantive findings but non-blocking; rewrite + re-submit at the same revision-grade. |
| `ACCEPT` | Ready (or ready with cosmetic nits). |

The team plots the two reviewers' verdicts across cycles. This is the
**verdict trajectory**.

## What a graduated paper's trajectory looks like

A well-functioning review loop produces monotone improvement on both
reviewers' columns:

| Cycle | Reviewer A | Reviewer B |
|---|---|---|
| v1 | MAJOR (6 A1) | REJECT (7 A1) |
| v2 | MAJOR (3 NEW A1) | REJECT (3 NEW A1) |
| v3 | MINOR (0 A1, 6 A2) | MAJOR (2 NEW A1) |
| v4 | MINOR / ACCEPT (0 A1, 1 A2, 3 A3) | MINOR for scope X / MAJOR for scope Y |
| v5 | MINOR / ACCEPT | MAJOR (2 NEW A1 from new content) |
| v6 | MINOR / ACCEPT | MINOR-to-MAJOR (2 NEW A1) |
| **v7** | **ACCEPT / GRADUATE** | **MINOR for both scopes** |

(Numbers are illustrative; this trajectory shape is typical for a
paper that graduates around cycle 7 after the L1.21 anti-pattern is
broken at v6→v7.)

Two telltales of a graduated paper:

1. **At least one cycle of UNANIMOUS ACCEPT** (or ACCEPT + MINOR with
   1–3 disclosed cosmetic nits).
2. **A monotone trajectory in the last 3 cycles** — verdicts
   improving or staying flat, no regressions caused by new revision
   content.

## When the trajectory regresses

Two patterns:

### Pattern 1 — L1.21 introduction

A revision closes one reviewer's MAJOR thread (verdict improves) but
introduces new bugs the other reviewer catches (verdict regresses).
This is the L1.21 anti-pattern (playbook 07). The fix is V1–V6
(playbook 06), not "try harder".

### Pattern 2 — Substantive new content

The revision folds a new experiment or expands a section. The new
content has its own bugs, and either reviewer flags them. This is
normal — new content always introduces some debt. The team should
expect 1–2 such cycles per substantive expansion, then return to the
monotone trajectory.

## The graduation criteria

A paper graduates when **all five** are true:

1. **Both reviewers ACCEPT** (or ACCEPT + MINOR with disclosed
   cosmetic nits) in the most recent cycle.
2. The two-cycle rolling history shows no regressions in either
   reviewer's verdict.
3. All A1-class findings from prior cycles are explicitly closed
   (referenced by close-review file path).
4. The reviewer-bait flag block (playbook 12) for the final revision
   is honest and substantive — 3–6 disclosed gaps, not token flags.
5. V1–V6 was run on the final revision and the output landed clean.

Two-cycle confirmation is required because a single ACCEPT can
coincide with a new-content cycle that simply hasn't introduced the
next-smallest bug yet. The L1.21 pattern produces single-ACCEPT
cycles that are not graduate-grade.

## The "first UNANIMOUS ACCEPT" milestone

In the seed session, the cold-tier paper reached its first UNANIMOUS
ACCEPT at cycle 12 (rev-8 of the paper). It was the **first**
unanimous accept in the entire 23-day session — the anchor paper had
been UNANIMOUS ACCEPT-equivalent at rev-20.1 cycle 7, but with one
microscopic cosmetic nit.

This is what graduation looks like operationally. It's also rare.
Most papers in most research projects ship at ACCEPT + MINOR cosmetic,
not ACCEPT + ACCEPT. UNANIMOUS ACCEPT is the floor of the discipline;
ACCEPT + MINOR is the typical graduation point.

## What "MINOR REVISIONS for scope X / MAJOR for scope Y" means

Reviewers can split verdicts by *scope*: "for an internal workshop
this is MINOR; for a top conference this is MAJOR". When the team
sees this:

1. Decide which scope you're aiming for. This is usually clear from
   the venue.
2. The verdict for the *target* scope is what you graduate on.
3. The other-scope verdict is information about how much further the
   paper would need to go for a stretch venue.

## Cycles-to-graduation

The seed session graduated two papers; both took **6–8 revisions**
plus **7 reviewer cycles** to graduate. Sample size of 2 papers is
too small to claim a general law, but the envelope appears to be:

- 3–4 cycles to get the substantive content in shape.
- 2–4 cycles to grind out L1.21 leftovers.
- Final 2 cycles at MINOR / ACCEPT or ACCEPT / ACCEPT.

If your paper has been at MAJOR for >5 cycles on the same finding,
that's a signal to either (a) apply productive falsification on the
finding (playbook 09) or (b) scope-limit the paper to exclude the
finding.

## When a paper does NOT graduate

Sometimes a paper genuinely cannot reach ACCEPT in the current scope.
Two acceptable outcomes:

1. **Re-scope.** Cut the contribution that's blocking. The remaining
   contribution may graduate at a different venue.
2. **Kill.** The contribution doesn't survive close review. This is
   rare but real — typically because a load-bearing mechanism story
   collapses under reviewer attack. Document the kill in the
   milestone log and learn from the structure of the failure.

What's **not** acceptable: shipping the paper at MAJOR REVISIONS by
ignoring the reviewers and submitting anyway. The reviewers' findings
were the cheap version of what readers will find after publication.

## Cost per graduation

The seed session's aggregate footprint per graduated paper was
approximately:

- ~25 experiments (cycles 1–N of multiple `EXP-NNNN`).
- ~15 paper revisions.
- ~9 reviewer cycles × 2 reviewers = 18 reviews.
- ~23 days of wall time, including productive falsification cycles.

For a load-bearing systems paper, this is **cheap** compared to the
cost of shipping a paper with a load-bearing error. For a quick
internal note or blog post, this is **expensive** and the discipline
should be scaled down: V1–V6 is still cheap; standing reviewer pair
is optional; cycles-to-graduation is 1–2 instead of 7+.

## The post-graduation snapshot

When a paper graduates:

1. Tag the research brain (e.g.,
   `git tag research-graduated-YYYY-MM-DD`) at the final commit.
2. Write a milestone log entry for the graduation.
3. The graduated paper is now **frozen at this revision**. Further
   discussion happens in `DISCUSSIONS.md` (playbook 03); the
   research brain stays at the tag.
4. If a discussion produces a finding that genuinely should
   supersede the paper, that finding lands in a *new* paper or in
   a corrigendum, not by silently editing the graduated revision.

## Anti-patterns

- **Self-graduating.** "We think this is good enough" without
  reviewer cycles is not graduation; it's submission. The reviewer
  pair is the gate.
- **Single-reviewer graduation.** ACCEPT from one reviewer +
  no-review from the other is not graduate-grade. Both reviewers,
  both ACCEPT.
- **Graduating on a regression-tail cycle.** If the last 2 cycles
  show verdict regression, the paper is not graduating yet — the
  L1.21 pattern is still active. Run another cycle with V1–V6.
- **"Final" never really being final.** If the team keeps revising
  after graduation, the tag and the discussion-brain consolidation
  rule (playbook 01) get violated. Once graduated, freeze.

## Connection to other playbooks

- The verdict trajectory is generated by **playbook 08** (the
  standing reviewer pair).
- The trajectory is *monotone* only when **playbook 06** (V1–V6) is
  run on every revision, because otherwise **playbook 07** (L1.21)
  introduces regressions.
- The team's reviewer-bait flag block (**playbook 12**) is part of
  the graduation criteria.
- Productive falsification (**playbook 09**) is what closes long-
  standing MAJOR threads when the standing reviewer keeps raising
  the same competing hypothesis.

Graduation is the **integration** of all the other playbooks. When it
goes smoothly, it is because the playbooks are running in concert.

---

*Origin: `data-pilot-research/paper/methodology-workshop-draft.md` § 7
("Quantitative Trajectory Analysis") and § 9 (Conclusion).*
