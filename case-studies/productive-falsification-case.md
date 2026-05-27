# Case study — productive falsification: refute the reviewer, find the mechanism

> **Source.** Two reviewer-hypothesis-refutation cycles from the seed
> session (`EXP-0050`-like and `EXP-0051`-like patterns). Specifics
> redacted; the pattern is what we extract.

> **Purpose.** Demonstrate playbook 09's claim: refuting a reviewer's
> hypothesis can advance the science as much as confirming the paper's
> claim — sometimes more.

---

## Case 1 — eliminating a contaminant

### The reviewer's hypothesis

A standing reviewer (the Claude variant in the seed session) reviewed
revision N of a paper that reported a positive headline residual under
a specific 3-component configuration. The reviewer noticed that one
of the three components in the configuration was *synthetic* — the
team's own generator — while the other two were drawn from
real-world corpora.

The reviewer's argument: the synthetic component was *the cleanest of
the three*. Substituting a real-world counterpart for the synthetic
component should make the residual **larger**, not smaller. If the
residual instead got smaller, that would mean the synthetic component
was acting as a contaminant — either too lossy or too lossless — and
the paper's headline claim was being inflated by the contaminant, not
by the underlying mechanism the team described.

The reviewer's prediction, paraphrased: "Substitute the real-world
counterpart and the residual will grow."

### The pre-registration

The team committed `HYPOTHESIS.md` with the reviewer's hypothesis as
the prediction:

- **H50.3:** Substituting the real-world counterpart for the synthetic
  component will produce a residual *larger* than the prior. ±30%
  band. Source of band: reviewer's structural argument.

The git-temporal commit happened *before* the substitution experiment
ran.

### The experiment

The team designed the experiment to test the reviewer's hypothesis on
its own terms: substitute the real-world counterpart, hold mass
weights constant, recompute the residual. No defensive design — no
attempt to make the synthetic component look better, no auxiliary
controls. Just the cleanest test of the reviewer's claim.

### The result

The reviewer's hypothesis was **refuted**. The residual *shrunk* by
a factor of 3.07× (from +15% to +5%). Direction: opposite of what the
reviewer predicted.

### The productive payoff

The refutation strengthened the paper materially. The team had been
worried that the synthetic component was somehow inflating the
headline — that's exactly what the reviewer suspected too. The
refutation showed that *the synthetic component had actually been
over-estimating* the contaminant effect, not under-estimating it.
With the real-world counterpart substituted, the headline residual
was 3× smaller than the prior paper revision had claimed. The unified-
store thesis got materially stronger, not weaker.

The paper's next revision:

- Replaced the synthetic-component anchor with the real-world-
  counterpart anchor (a tighter, more defensible number).
- Documented the substitution methodology as a reusable cross-check
  in the supplement.
- Cited the 3× residual-magnitude reduction as a paper-strengthening
  finding.

**The methodology the reviewer motivated was correct, even though the
direction the reviewer predicted was wrong.** This is the productive
falsification pattern: the reviewer's framing forced the experiment
to be designed as a *cross-check*, and the cross-check's result —
opposite-direction — was more informative than a directional confirmation
would have been.

---

## Case 2 — refuting a linear hypothesis, discovering a piecewise mechanism

### The reviewer's hypothesis

A different standing reviewer (the GPT variant) reviewed a separate
paper's closed-form mechanism model for 5 consecutive cycles. The
reviewer's argument across all 5 cycles: a specific axis (call it
`K`) was load-bearing in a way the closed-form was missing. The
reviewer proposed a **linear additive term**: cost should scale with
`K` at a magnitude of "50–100 units per K".

Across 5 revisions, the reviewer held the paper at MAJOR REVISIONS
until this term was either confirmed or refuted on independent
variation of `K`.

### The pre-registration

The team committed `HYPOTHESIS.md`:

- **H51.1:** Linear additive term in `K`, at 50–100 units per `K`,
  ±band. Source: reviewer's mechanism argument across 5 cycles.

The experiment was designed to decouple `K` from the axes it had
been collinear with in prior experiments. A 45-cell grid: 3 levels
of axis A × 3 levels of axis B × 5 levels of `K`.

### The result

The fit returned **a negative slope** with magnitude three orders of
magnitude off the reviewer's prior. Wrong direction (the term went
negative, not positive); wrong magnitude (the slope was at the
single-byte scale, not the kilobyte scale).

**H51.1 REFUTED with very high confidence.**

### The productive payoff

This is where the pattern earns its name. With the linear hypothesis
killed, the team re-examined the residual structure cell-by-cell. The
residuals revealed a **piecewise structure** at a natural boundary
in the system's underlying mechanism. Below the boundary: one regime
with one set of constants. Above the boundary: a different regime
with the existing closed-form *plus* an additional per-block term.

A previously-failing out-of-sample cell that had been at −16.86%
under the old model (a soft-fail) became +3.35% PASS under the new
piecewise model.

The piecewise structure was the **load-bearing finding** of the
paper. It was only discoverable because:

1. The reviewer's `K`-axis hypothesis was pre-registered, forcing
   the experiment to vary `K` independently.
2. Variation in `K` decoupled it from the axes it had been collinear
   with, exposing the boundary the piecewise structure rests on.
3. The refutation made the team look at the residuals cell-by-cell
   instead of accepting the prior closed-form's "R² = 0.9992"
   summary statistic.

Had the reviewer not held the paper at MAJOR for 5 cycles on the
`K`-axis hypothesis, the team would not have run the experiment with
`K` as an independent axis; without `K` as an independent axis, the
piecewise boundary would have stayed collinear with the size axis
`N` and remained invisible.

The paper's closure-thread (5 consecutive MAJOR cycles) closed on the
next revision. The paper graduated UNANIMOUSLY shortly thereafter
(see `case-studies/L1.21-case-2.md` for the graduation cycle).

---

## Generalisation

In both case studies the pattern is:

1. A reviewer-proposed hypothesis is **committed as a pre-registration**
   before the experiment runs.
2. The experiment is designed to **test the hypothesis on its own
   terms** — not to defend the paper's current claim against it.
3. The hypothesis is **refuted**.
4. The refutation, because the experiment was designed around the
   hypothesis, exposes the **actual mechanism** (case 2) or
   eliminates a **contaminant** (case 1).

The crucial design choice is step 2: the experiment must be **structured
to test the reviewer's claim on its own terms**, even if doing so feels
like it concedes the reviewer's framing.

## Why "productive" not "defensive"

The defensive instinct — design the experiment to make the current
claim look good *against* the reviewer's hypothesis — misses the
payoff. Defensive designs cluster around the prior claim and don't
span the reviewer's predicted axis. Without spanning the axis, the
refutation (if it comes) can't reveal the residual structure that
*was* hidden behind the reviewer's proposed mechanism.

In case 1, a defensive design would have been "rerun the original
configuration with tighter CIs and show the residual is still in
range". This would have addressed the reviewer's *severity* but not
their *concern*. The reviewer's concern was about the **identity** of
one of the components; a defensive design would have re-measured but
not substituted.

In case 2, a defensive design would have been "show that `K` has no
detectable effect" — likely measured by running 2 cells at extreme
`K` values. This would have *technically* refuted the reviewer's
hypothesis but at low resolution; the piecewise boundary, which sits
in the middle of `K`'s range, would have remained invisible.

The productive-falsification design spans the disputed axis, gives
the reviewer's hypothesis a clean test, and as a side-effect *also*
reveals structure that no other design would have surfaced.

## What the case studies do NOT show

These two case studies are wins. The pattern also has losses:

- Sometimes the reviewer's hypothesis is *correct*, and the
  experiment confirms it. The paper has to be substantially
  revised in the reviewer's direction. This is fine — the science
  has advanced — but it's not "productive falsification" in the
  sharp sense.
- Sometimes the refutation is *non-mechanistic* — the residuals look
  like noise, no new mechanism appears. The reviewer's thread closes
  but the paper isn't materially strengthened. This costs a cycle
  with limited payoff.

The seed session's two case studies above are the productive ones;
both contributed paper-headline-load-bearing findings. The
non-productive cases also occurred but did not generate quotable
findings.

## When to apply this pattern

| Condition | Apply? |
|---|---|
| Reviewer proposes a hypothesis that **contradicts** a current paper claim | **Yes** |
| Reviewer holds the paper at MAJOR for ≥3 cycles on the same point | **Strongly yes** |
| Reviewer proposes a hypothesis that **supplements** a current claim | Maybe; pre-register and test, no harm |
| Reviewer raises a methodological concern with no specific competing claim | No; address methodologically |

---

*Source: `data-pilot-research/paper/methodology-workshop-draft.md`
§ 6 ("Productive Falsification as Research Strategy"); seed-session
`EXP-0050` and `EXP-0051` blocks in `ITERATIONS.md`. Specifics
paraphrased; domain stripped.*
