# Playbook 09 — Productive Falsification as Research Strategy

> When a reviewer proposes a hypothesis that competes with your claim,
> design an experiment that **tests the reviewer's hypothesis on its own
> terms**. The refutation, if it comes, will usually reveal the *actual*
> mechanism — which had been hidden behind the reviewer's proposed one.

## The conventional framing — and why it's incomplete

Pre-registration (playbook 05) is usually framed as a *defence* against
HARKing-after-the-fact. The defence story:

> "We commit our prediction *before* the measurement. If we change it
> after, the git graph catches us. Therefore our claims are honest."

That's true but it misses the more powerful use of pre-registration: as
a **productive falsification engine**.

## The productive-falsification pattern

The pattern in five steps:

1. **A reviewer proposes a hypothesis** that competes with the paper's
   current claim. Often the reviewer holds the paper at MAJOR
   REVISIONS for multiple cycles on this hypothesis.
2. **The team pre-registers the reviewer's hypothesis as a prediction**,
   with a band and a falsification criterion, in `HYPOTHESIS.md`.
3. **The team designs an experiment to *test* the reviewer's hypothesis**
   — not to defend the current claim against it. The axes, the grid,
   the sample design are all chosen to give the reviewer's hypothesis
   the cleanest possible test.
4. **The experiment refutes the reviewer's hypothesis** with high
   confidence.
5. **The refutation reveals the actual mechanism.** Because the
   experiment was designed around the reviewer's claim — with axes the
   claim implied — the refutation typically exposes the structure that
   had been hidden behind the proposed (wrong) explanation.

The crucial design choice is step 3. The amateur reaction is "let me
design an experiment that defends the paper's current claim against the
reviewer." The disciplined reaction is "let me design an experiment
that the reviewer's claim would *clearly pass* if it were true."

## Why this works

Three reasons:

### 1. The reviewer's framing tightens the experiment

A reviewer who has held a paper at MAJOR for multiple cycles has done
work for you: they've identified an axis you didn't isolate. When you
design the experiment to test their hypothesis, you implicitly decouple
that axis from whatever it was coupled to in your prior experiments.
The decoupling often reveals structure that was collinear with another
axis in the original design.

### 2. Refutation is more informative than confirmation

If the experiment *confirms* the reviewer's hypothesis, you update the
paper and the science advances. Fine. But if the experiment *refutes*
it, the refutation tells you specifically:

- What the magnitude of the wrong-prediction is.
- What sign it's in (the reviewer thought +X; reality is −Y).
- Where the residuals lie.

The residuals are where the mechanism reveals itself. They wouldn't be
visible if you'd designed the experiment defensively — defensive
designs cluster around the prior claim and don't span the reviewer's
predicted axis.

### 3. The team is now bought in

Once the reviewer's hypothesis is in `HYPOTHESIS.md`, the team can
no longer dismiss it as "the reviewer doesn't understand". The team
has committed to a test. When the refutation lands, the resulting
finding has the reviewer's framing baked into the audit trail. The
paper gets stronger because it has *survived* the most serious
competing hypothesis a knowledgeable critic could pose.

## The abstract case study

(For two specific case studies, see `case-studies/productive-
falsification-case.md`. Here we describe the *pattern* without the
domain specifics.)

**Setup.** A reviewer held a paper at MAJOR REVISIONS for five
consecutive cycles. The standing critique: the paper's closed-form
model was missing a term that depended on a specific axis. The reviewer
proposed a *linear* relationship at a specific magnitude.

**Pre-registration.** The team committed `HYPOTHESIS.md` with:

- `H1`: linear relationship at [reviewer's proposed magnitude] ±band.
- Falsification criterion: any other functional form (constant, quadratic,
  piecewise) is REFUTED in favour of `H0` (no linear term).

**Experiment design.** A grid decoupled the disputed axis from the
axis it had been collinear with in prior experiments. Cells: three
levels of axis A × three levels of axis B × five levels of the
disputed axis.

**Result.** The fit returned a negative slope, three orders of
magnitude off the reviewer's prior. `H1` REFUTED with very high
confidence.

**The productive payoff.** With the linear hypothesis killed, the
cell-by-cell residuals revealed a **piecewise structure** at a natural
boundary in the system. Below the threshold: one regime. Above: a
different regime, with an additional per-block term. The piecewise
structure was only discoverable because the disputed axis was now
independent — and the reviewer's hypothesis was what motivated isolating
it. The actual mechanism had been collinear with another axis in the
original design and therefore invisible.

**Closure.** The standing 5-cycle MAJOR thread closed at the central
observable level (the previously-failing out-of-sample cell went from
−16.86% (soft-fail) to +3.35% (PASS) under the new piecewise model).
The reviewer's specific hypothesis was wrong, but the *axis* the
reviewer identified was correct — the team just hadn't isolated it
yet.

## When to apply this pattern

| Condition | Action |
|---|---|
| Reviewer proposes a hypothesis that contradicts a current paper claim | Apply productive falsification |
| Reviewer proposes a hypothesis that *supplements* a current claim | Pre-register and test, but defensive-design is also fine |
| Reviewer raises a methodological concern (no specific competing claim) | Address methodologically (re-measure, re-scope); no productive-falsification framing |
| Reviewer holds the paper at MAJOR for ≥3 cycles on the same point | **Strong signal** to apply productive falsification |

## When it backfires

Two failure modes:

### 1. The reviewer's hypothesis is right

The experiment confirms the reviewer's proposed mechanism. You now
have to incorporate it into the paper, often substantially. The paper
gets longer and the contribution gets re-scoped. This is fine —
"reviewer was right" is a normal outcome — but it does push the paper
toward the reviewer's framing.

### 2. The refutation is non-mechanistic

The reviewer's hypothesis is refuted but the residuals don't reveal a
new mechanism — they look like noise. In this case the refutation
*closes* the reviewer's thread but doesn't strengthen the paper. The
paper still has to explain what's actually going on. This happens
when the disputed axis really was a red herring.

In either failure mode, the experiment cost you a cycle. That cost is
small compared to the cost of shipping a paper that loses to the
reviewer's competing hypothesis post-publication.

## Anti-patterns

- **Defensive experiment design.** Designing the experiment to make
  the current paper claim look good *against* the reviewer's
  hypothesis, rather than to give the reviewer's hypothesis a clean
  test. This misses the productive-falsification payoff.
- **Re-stating the reviewer's hypothesis weakly.** If the band is
  wide enough that any result will technically PASS, the experiment
  isn't a real test. Use the band the reviewer would set, not the
  band that's safest for you.
- **Post-hoc band widening on REFUTED.** If the reviewer's hypothesis
  is refuted at the pre-registered band, the band is the band. Don't
  widen it after the fact. The refutation is the finding.
- **Skipping the pre-registration commit.** If the experiment runs
  before the prediction is committed, the productive-falsification
  framing is unavailable — the git graph can't demonstrate that the
  refutation was on the reviewer's terms.

## Productive falsification as a research strategy

In the seed session, two of the strongest findings — both
paper-headline-load-bearing — came from refuting reviewer hypotheses:

- One case eliminated a contaminant in a prior result, materially
  strengthening the headline claim (a 3× reduction in residual
  magnitude).
- One case directly led to the discovery of a piecewise-by-block
  mechanism that was the load-bearing finding of the cold-tier
  companion paper.

**Refuting reviewers' hypotheses produced findings stronger than
confirming the team's own.** This is the case for taking competing
hypotheses seriously, pre-registering them as your own predictions, and
designing experiments that give them the cleanest possible test.

---

*Origin: `data-pilot-research/paper/methodology-workshop-draft.md` § 6
("Productive Falsification as Research Strategy"). The two case studies
are abstracted in `case-studies/productive-falsification-case.md`.*
