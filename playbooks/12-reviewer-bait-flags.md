# Playbook 12 — Reviewer-Bait (Honest-Flag) Disclosure

> At the end of every paper revision, the team itself catalogues the
> revision's honest weaknesses — the gaps a hostile reviewer would
> attack — and lists them in a "reviewer-bait flags" block. Self-
> disclosed weakness costs a paragraph; reviewer-found weakness costs
> a cycle (or a retraction).

## The principle

A paper that lists its own weaknesses in a clearly labelled "honest
flags" block does two things:

1. **Pre-empts reviewer findings.** Anything you self-disclose, the
   reviewer doesn't have to discover. The reviewer's attention
   redirects to issues you haven't seen.
2. **Calibrates the reader.** A paper that says "here are our gaps"
   is harder to dismiss as overclaiming. The reader trusts the
   non-gap claims more, because the team has demonstrated the
   discipline to mark what's not solid.

The amateur instinct is to hide weaknesses. The disciplined practice
is to surface them, label them, and let the reader weight them.

## Where the flag block lives

In a paper draft, the flag block sits at the bottom of the relevant
section — often as a final paragraph titled **"Honest reviewer-bait
flags for rev-N"** or similar. The seed session put it in the
supplement, one per revision, accumulating across cycles.

The block does **not** replace the paper's formal threats-to-validity
section. Threats-to-validity is for *systemic* limitations
("our experiments use synthetic data; we have not validated on
production traces"). Reviewer-bait flags are for *this-revision-
specific* honest weaknesses — the things this revision could be
attacked for, even if a future revision will address them.

## What goes into a flag

Each flag is:

- **One sentence** stating the weakness.
- **A second sentence** anticipating the hostile reviewer's framing.
- Optionally, a third sentence pointing to mitigations queued for
  future revisions.

Example structure (domain-neutral):

```markdown
- **(a) The new experiment is single-seed at one operating point.**
  The headline residual passes the ±band by ~6× margin, but it is
  not a CI'd interval. A hostile reviewer can ask for n=3 seed CI
  on the headline cell. Queued as EXP-NNNN.

- **(b) The configuration mix is fixed at equal weights.**
  Heavily-skewed weight configurations are unmeasured and could
  amplify the column-sum residual we report. A hostile reviewer can
  ask for the skewed-weight case as a falsifier.

- **(c) The new term in the closed-form is single-seed at three
  cells.** Two of the three cells are near-threshold; a hostile
  reviewer can ask whether the closed-form's confidence interval
  actually covers them.
```

## How to generate flags

After each revision, the agent (or team) **simulates a hostile reviewer**
and lists 3–6 issues they would raise. The simulation should target:

| Class | Question to ask |
|---|---|
| **Sample size** | Is any headline claim single-seed? Single-cell? Single-corpus? |
| **Generalisation** | Does the claim's scope match the evidence's scope? |
| **Convention** | Have you stated denominator conventions, normalisation choices, baseline definitions? |
| **Mechanism** | Is the mechanism story falsifiable? Has it been falsified? |
| **Cross-section** | Are constants used in this section the same as constants used elsewhere? |
| **Counter-claims** | Does any older section of the paper still assert something the new section refutes? |

Each "yes, that's a real gap" becomes a flag.

## How flags accumulate across revisions

The flag block from each revision **stays in the supplement** — it
isn't deleted when a future revision addresses the flag. The audit
trail is:

- rev-17 flag (a): "experiment X is single-seed."
- rev-18 closes flag (a) by running n=3 (note this in rev-18's
  changelog).
- rev-18 introduces *new* flags (a'), (b'), (c') that the rev-18
  diff created.

The reader can follow the flag trail across revisions and see the
team's calibration improving over time.

## The hostile-reviewer simulation in `MECHANISM.md`

Each experiment's `MECHANISM.md` includes a hostile-reviewer
simulation as a section. This is the *experiment-level* analogue of
the *paper-level* flag block:

```markdown
## Hostile-reviewer questions

(1) Why is the residual non-monotone in axis X?
ANSWERED — the non-monotonicity is artifact of a cycle-0 axis
mis-labelling; see § 0. Real residual is monotone.

(2) Is single-seed enough for a paper-headline anchor?
For PASS/FAIL of pre-registered Hi, yes; the singletons are
writer-deterministic vs the prior anchor. n=3 CI is queueable
but not a blocker.

(3) What about scale Y, which you didn't measure?
OPEN. Inherited from the prior experiment's scope caveat.
Queued for EXP-NNNN.

(4) ...

(N) ...
```

The hostile-reviewer simulation should run to **6–10 questions**.
Stop when the simulation is producing diminishing-quality questions
(typos, formatting). The 6–10 strongest become flags or paper
disclosures.

## Why this works

Three mechanisms:

### 1. Honesty is cheaper than embarrassment

A self-disclosed flag costs a sentence. A reviewer-discovered flag
costs a review cycle (~weeks of wall time) and a credibility hit.
The cost ratio is enormous. The team that flags reliably pays a
small constant overhead; the team that hides pays unbounded
catastrophic overhead.

### 2. Flags redirect reviewer attention

A reviewer has finite attention. Anything you self-disclose, the
reviewer skims past on the way to *the things you didn't disclose*.
That redirection is itself valuable — it concentrates the
reviewer's effort on unknown unknowns, where the team most needs
help.

### 3. The flag block trains the team

After 5 revisions of writing flag blocks, the team starts to
*pre-anticipate* the flags during the writing of the next
revision. The discipline becomes internalised: the agent writes
the new section already considering "what would a reviewer ask
about this?" — and writes the section to forestall the question.

## Anti-patterns

- **Token flags.** Listing one flag for the sake of having "honest
  flags". Real practice requires 3–6 substantive flags per major
  revision.
- **Vague flags.** "Our experiments could be more comprehensive" is
  not a flag. "Our experiments are single-seed at scale = 1M; CV
  unknown above 100M" is a flag.
- **Defensive flags.** Stating a flag and then immediately denying
  it ("(a) We use synthetic data — but it's calibrated, so it's
  fine"). The reviewer sees through this. Either you genuinely
  consider it not-a-gap (in which case it doesn't need a flag) or
  it's a gap (in which case state it cleanly).
- **Flags only after MAJOR.** Treating the flag block as
  damage-control after a bad review. The flag block should appear
  on every revision, including the first.
- **Conflating threats-to-validity and flag block.** Threats are
  for systemic limitations; flags are for revision-specific honest
  weaknesses. Keep them separate.

## Sample seed-session flag block (abstracted)

(From the seed session's anchor paper at the cleanest revision.
Specific numbers stripped; pattern preserved.)

> **Honest reviewer-bait flags for rev-N.**
>
> (a) The new corpus's specific scale is structural-shape-
> representative but cardinality-conservative; a hostile reviewer
> can ask "what about at production scale?" — queued as EXP-NNNN.
>
> (b) The mix ratio is fixed equal-weighted at three components;
> skewed mix ratios are unmeasured and could amplify the headline
> residual.
>
> (c) The neighbourhood of operating points is single-anchor at
> several pillar/condition combinations; a reviewer can ask for
> wider neighbourhood coverage.
>
> (d) The supersession of the previous slope estimate is a "we
> corrected a number you trusted" disclosure. The 2-point → 6-point
> regression supersession is honest, but a reviewer can still cite
> it as evidence of "the team consistently mis-measures first and
> corrects under reviewer pressure".
>
> (e) Our claim's scope is bounded to specific configurations; a
> reviewer can ask whether the claim generalises to alternative
> configurations — open question.

Note the structure: each flag states the weakness, then anticipates
the hostile framing. The framing matters as much as the disclosure —
it shows the team has considered the rhetorical attack, not just the
technical one.

---

*Origin: `data-pilot-research/paper/SUPPLEMENT.md` "Honest reviewer-
bait flags for rev-N" blocks across multiple revisions; the
hostile-reviewer simulation convention in the seed session's
per-experiment `RESULTS.md` files.*
