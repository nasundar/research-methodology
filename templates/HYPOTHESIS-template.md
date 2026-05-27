# EXP-NNNN — `<one-line title>`

> **Git-temporal status.** This is a prediction-only commit. Measurement
> and result land in a later commit per playbook 05 (git-temporal rule).

## § 0 Cycle-0 knob-realisation audit

For each parameter being swept, emit `intended=X, realised=Y, drift=Z%`.
If any |Z| > 5 %, redefine the axis on the realised value or fix the
emit-side semantics before proceeding to § 1.

| Knob | Intended | Realised | Drift | Action |
|---|---|---|---|---|
| <knob1> | … | … | …% | OK / FIX |
| <knob2> | … | … | …% | OK / FIX |

(If any rows are FIX: do not proceed to § 1 until the FIX rows are
resolved. See playbook 11.)

## § 1 Pre-registered hypotheses

- **H<X>.1**: <quantitative claim>. Predicted direction: <+/-/=>.
  Predicted magnitude: <value ± band>. Source of the band:
  <prior measurement / theory / reviewer suggestion>.
- **H<X>.2**: ...
- **H<X>.3**: ...
- **H<X>.4**: ...

## § 2 Falsification criterion

For each Hi above, state the operational definition of REFUTED:

- **H<X>.1 REFUTED if:** <measurable condition>. <e.g., "measured
  reduction lies outside [15%, 45%], or sign flips">.
- **H<X>.2 REFUTED if:** ...

## § 3 Sample-set + seed plan

- **Seeds:** n = <3 minimum for paper-headline-load-bearing>; CI
  strategy: paired-ratio.
- **Sample-set:** fixed at <description>. Cross-cycle sample-set
  drift disclosure if applicable.
- **Mechanism probe (if applicable):** <which probe will measure the
  mechanism, not just infer from the headline metric>.

## § 4 Reviewer-proposed hypotheses being tested (optional)

(If this experiment is testing a reviewer's hypothesis from the
standing review pair, cite the close-review file path and line. This
enables productive falsification per playbook 09.)

- Reviewer A v<N> A1-<n>: <paraphrase>. Pre-registered here as
  H<X>.1 above with band <±N%>.

## § 5 Predicted outcome (decision table)

| Outcome scenario | Decision |
|---|---|
| All Hi PASS within band | Advance; consolidate in paper § N |
| Hi REFUTED but mechanism falsifier intact | Productive falsification (playbook 09); refit mechanism |
| Cycle-0 reveals knob drift > 5% | Pause; fix; re-pre-register |
| Surprise direction (sign flip) | Treat as new finding; new hypothesis cycle |

## § 6 Iteration plan (for cycles ≥ 2)

(If this is cycle 2+, summarise what cycle 1 found and what this
cycle changes. State explicitly: "this cycle changes the sample set",
"this cycle adds an axis", "this cycle pins the prior CI", etc.)

---

*Origin: `data-pilot-research/paper/methodology-workshop-draft.md`
Appendix C ("HYPOTHESIS.md skeleton") and the seed session's
`experiments/00NN-*/HYPOTHESIS.md` convention.*
