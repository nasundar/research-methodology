# Playbook 05 — Hypothesis-Driven Research

> Every experiment begins with a written hypothesis, committed *before*
> the measurement is run. This is the foundation of falsifiability, of
> productive falsification, and of audit-trail integrity.

## The core rule

For every experiment `EXP-NNNN`, the following must be true:

1. `experiments/NNNN-<slug>/HYPOTHESIS.md` exists.
2. It contains one or more numbered hypotheses (`HX.1`, `HX.2`, …) with
   **quantitative bands** and **falsification criteria**.
3. The `HYPOTHESIS.md` is committed in a commit that **precedes** the
   commit containing the measurement results, so `git log --oneline --
   experiments/NNNN-*/` shows prediction-before-measurement on the
   commit graph.

This is the **git-temporal rule**: prediction is only credible if it
predates measurement, and the commit graph is the cryptographic record.

## Why pre-registration

Three independent reasons:

### 1. Prevents HARKing (Hypothesising After Results are Known)

The natural pattern of an unprepared researcher is: measure, then write
a hypothesis that fits the measurement. This isn't dishonesty — it's
human cognition. Pre-registration removes the temptation by making the
sequence inverted-and-cryptographic: the prediction is committed
*before* the result is observable.

### 2. Enables productive falsification

A pre-registered hypothesis is a target. When a reviewer proposes a
competing hypothesis (e.g., "I think the residual will scale linearly
with X"), you can pre-register the reviewer's claim *as your own
prediction* with a band and a falsification criterion. The experiment
then tests the reviewer's hypothesis on its own terms. If refuted, the
refutation typically reveals the actual mechanism. See playbook
**09-productive-falsification** for the full pattern.

### 3. Makes "but why?" cheap

A hypothesis includes a *direction* and a *magnitude*. When the
measurement comes back, you can read off whether each (direction,
magnitude) bet PASSED, FAILED, or surprised. The surprise cells are
where the research lives. Without a pre-registered prediction, there
are no surprise cells — there's just a number.

## What goes in `HYPOTHESIS.md`

Use the template at `templates/HYPOTHESIS-template.md`. The required
sections:

| § | Section | Purpose |
|---|---|---|
| 0 | Cycle-0 knob-realisation audit | Verify your knobs actually do what you think (playbook 11). |
| 1 | Pre-registered hypotheses | `HX.1` … `HX.N` with quantitative bands. |
| 2 | Falsification criterion | What outcome would refute each `HX.i`? |
| 3 | Sample-set + seed plan | n ≥ 3 seeds; sample-set fixed; CI strategy. |
| 4 | Reviewer-proposed hypotheses being tested | (Optional but high-value.) |

### Section 1 — hypothesis statement

Each hypothesis is **quantitative and bounded**:

```markdown
- **H1**: <claim>. <Quantitative prediction with ±band>. Source of
  the band: <prior measurement / theory / reviewer suggestion>.
```

Example (domain-neutral):

```markdown
- **H1**: Variant A reduces the headline metric by 30% [±15%] vs
  variant B at the canonical scale point N. Band source: order-of-
  magnitude estimate from the mechanism in `notes/m2-mechanism.md`.
```

### Section 2 — falsification criteria

For each `HX.i`, state the **operational definition of REFUTED**:

```markdown
- H1 REFUTED if: measured reduction is outside [15%, 45%] under
  the same configuration (or sign flips).
```

Bands are not vibes. They come from one of:

- **Prior measurement** on a related configuration (the most common).
- **Theory** (an analytic derivation of expected magnitude).
- **Reviewer suggestion** (in which case label it explicitly so the
  productive-falsification pattern can run).
- **Deliberately falsifiable prior** — set wide so that *any* refutation
  is informative. Useful when there's no good prior.

### Section 3 — sample-set + seed plan

The seed promotion gate:

- `n ≥ 3` seeds for any paper-headline-load-bearing claim.
- Paired-ratio CIs (or carry a CV from a same-harness prior
  measurement on the same configuration).
- Sample-set fixed *and* disclosed. If a later cycle changes the
  sample set, the cross-cycle comparison is not apples-to-apples and
  must be explicitly flagged.

### Section 4 — reviewer-proposed hypotheses (optional)

If this experiment is testing a reviewer's hypothesis, cite the
close-review file path and line number. This enables the productive-
falsification pattern (playbook 09).

## The commit pattern

```bash
# Step 1: write the prediction-only commit. RESULTS.md may be empty
# or contain only a "pending" placeholder.
git add experiments/NNNN-<slug>/HYPOTHESIS.md
git commit -m "EXP-NNNN cycle-K: prediction (results pending)"

# Step 2: run the measurement. This is local activity; no commit.

# Step 3: commit the results in a separate commit.
git add experiments/NNNN-<slug>/RESULTS.md experiments/NNNN-<slug>/MECHANISM.md
git commit -m "EXP-NNNN cycle-K: result + mechanism reconciliation"
```

After this, `git log --oneline -- experiments/NNNN-*/` shows the
prediction commit precedes the result commit. The commit graph is the
audit trail.

## Common pitfalls

### Pitfall 1 — Vague bands

`H1: variant A is faster than variant B` is **not** a hypothesis. It
has no magnitude and no band. The minimum acceptable statement is:
`H1: variant A is at least X% faster than B at the canonical
configuration, ±Y%`.

### Pitfall 2 — Predicting at the wrong granularity

If your falsification criterion is "the residual is within ±25%", make
sure the experiment can measure residuals at that resolution. A
50%-CV result cannot resolve a 25% band.

### Pitfall 3 — Post-hoc band widening

If the result lands outside the original band, do **not** widen the
band in `HYPOTHESIS.md` after the fact. The prediction is what it was.
Either accept the REFUTED verdict and report it, or re-state the
hypothesis explicitly in a *later* `HYPOTHESIS.md` (e.g., `HZ.1`) at
the new band and re-test. The git graph records both.

### Pitfall 4 — One hypothesis at a time

`HYPOTHESIS.md` can carry multiple hypotheses (`HX.1` … `HX.5`).
Don't compress them into one — each must be independently testable
and independently falsifiable.

### Pitfall 5 — Skipping the cycle-0 audit

Section 0 of `HYPOTHESIS.md` is the cycle-0 knob-realisation audit
(playbook 11). Without it, you may be testing a hypothesis on an axis
that doesn't mean what you think it means. See the seed session's
"13× phantom" case study referenced from playbook 11.

## What if there's no good prior?

Sometimes you genuinely don't know the magnitude. Two options:

1. **Run a smoke experiment first.** Label it `EXP-NNNNa — smoke` and
   do not write quantitative hypotheses. Use the smoke result to set
   the band for `EXP-NNNNb` (the real experiment), which has a
   normal `HYPOTHESIS.md`.
2. **Set a deliberately wide falsifiable prior.** Document it as
   such: `H1: <claim> at <very wide band>. The band is deliberately
   wide to test whether the experiment surfaces an unrecognised
   mechanism.`

Both are honest. The dishonest option — measuring first and then
writing a "predicted X" matching the result — is what pre-registration
exists to prevent.

---

*Origin: `data-pilot-research/AGENT_WORKTREE_PROTOCOL.md` rule 5b
(git-temporal prediction) and rule 5c (n ≥ 3 seed promotion gate);
the seed session's `HYPOTHESIS.md` files under
`experiments/00NN-*/HYPOTHESIS.md`; `paper/methodology-workshop-draft.md`
Appendix C.*
