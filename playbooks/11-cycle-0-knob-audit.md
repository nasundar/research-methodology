# Playbook 11 — The Cycle-0 Knob-Realisation Audit

> Before any parameter sweep, verify that the parameters you think you're
> sweeping are actually the parameters the system observes. Any drift
> >5% between intended and realised values is a **methodology bug, not
> a finding** — and must be fixed before measurement, not interpreted
> after.

## The principle

Every experiment that varies a knob has, implicitly, two values for
that knob:

- **Intended** — the value the experimenter set in the configuration
  ("I asked for a mix of 50% A and 50% B").
- **Realised** — the value the system actually observed
  ("the system actually saw 90% A and 10% B because of upstream
  amplification").

When intended ≠ realised, the experiment's axis is *mis-labelled*. The
chart says "50% A vs 50% B"; the data is at "90% A vs 10% B". Any
mechanism story derived from the chart is wrong — sometimes
dramatically wrong.

This failure mode is the **knob-vs-realised confusion**. It happens
when there's a transformation between the configuration knob and the
quantity the system actually measures, and the transformation is
non-trivial (a multiplier, a quota, a saturation, a rounding).

## The seed session's case: the 13× phantom

In the seed session, a parameter sweep claimed to show a 13× cost
spread across different mix configurations. Two full cycles of
iteration plus an entire mechanism-audit experiment were spent
investigating the mechanism behind the 13× spread.

The spread was an artifact. The mix axis was defined on *emitted
events*, but an upstream amplification multiplied one of the components
by ~10× before the writer saw it. So the realised mix was wildly
different from the intended mix, and the chart's "13× spread across
mixes" collapsed to **1.0× spread** when re-plotted on realised-mix.

Two cycles of work, plus the entire mechanism audit, produced
nothing — the finding was the size of the methodology bug. Without
the cycle-0 audit being formalised, the paper would have shipped a
13× artifact as a flagship result.

The full case is preserved in `case-studies/L1.21-case-2.md` (the
specific story is referenced; the *pattern* — knob-vs-realised
confusion as a recurring failure mode — is what we focus on here).

## The cycle-0 audit, operationally

Before *any* depth-iteration cycle begins on a parameter sweep:

1. For every knob being swept, the experiment harness emits **both**
   the intended value and the realised value (the value the system's
   internal observer sees).
2. The agent computes the drift: `Z = (realised - intended) / intended`.
3. If `|Z| > 5%` for any knob:
   - The experiment is **paused**.
   - The team chooses one of two responses:
     - **(a) Redefine the axis** on the realised value. Re-label
       charts, re-state hypotheses, re-design the grid if needed.
     - **(b) Fix the knob's emit-side semantics.** Either the system
       has a bug (most cases) or the configuration was misused.
       Fix at the source.
4. The audit lands in `HYPOTHESIS.md § 0` as a 1-line summary per knob:
   `intended=X, realised=Y, drift=Z%`.

## Why this is "cycle 0", not "cycle 1"

The audit must be **before** the measurement, because:

- After measurement, the agent is tempted to interpret a drifted axis
  *as if it were the intended axis*. This is L1.21-flavoured — a
  literal axis-labelling bug in the chart.
- Before measurement, the audit is cheap (one printout per knob).
- A drifted axis discovered post-measurement requires the whole
  measurement to be re-run on the corrected axis. This is the
  expensive case.

So cycle 0 sits *before* the sweep proper. Cycle 1 is the first
measurement on the validated axes.

## When the audit is non-trivial

Some knobs are easy to audit (just print the value the writer sees).
Some are not — when the realised value depends on a complex chain of
upstream transformations, the audit may itself require a small
experiment. That's fine; the cost of even a half-day cycle-0 audit is
small compared to the cost of a multi-cycle phantom investigation.

Two common patterns:

### Pattern 1 — Quota / amplification

The configuration says "process N items"; the system queues, batches,
or amplifies, and the writer sees `k·N` items. The audit prints both.
If the amplification factor `k` was supposed to be 1.0 but is 10.0,
that's the bug.

### Pattern 2 — Saturation

The configuration says "spread the workload across K parallel slots";
the system saturates at fewer slots than configured (because resources,
locks, or upstream parallelism limit). The audit prints both
intended K and realised K. If realised K is much smaller than
intended K, the experiment isn't measuring what it thinks.

### Pattern 3 — Rounding / truncation

The configuration is a float; the system uses an integer. The
configuration is a fraction; the system rounds to the nearest discrete
slot. The audit prints both.

## The audit's place in `HYPOTHESIS.md`

```markdown
## § 0 Cycle-0 knob-realisation audit

For each parameter being swept, emit `intended=X, realised=Y, drift=Z%`.
If any |Z| > 5%, redefine the axis on the realised value or fix the
emit-side semantics before measuring.

| Knob | Intended | Realised | Drift | Action |
|---|---|---|---|---|
| <knob1> | 50 | 49 | -2% | OK; sweep on intended |
| <knob2> | 50 | 5 | -90% | FIX; emit-side bug in <component> |
| <knob3> | 0.5 | 0.5 | 0% | OK |

(if any FIX rows: do not proceed to § 1 hypotheses until the FIX rows
are resolved)
```

## What about non-knob axes?

The audit is for **swept knobs**. Constant configuration parameters,
seed values, and global settings don't need a per-cycle audit — they
need to be **declared once** in the experiment's setup section and
verified that they didn't drift since the last experiment.

But: any parameter you think is constant *and is actually shared with
the parameter you're sweeping* is a candidate for accidental
confounding. If a knob you're sweeping is "the number of inputs" and
the constant you've declared is "the input size", verify that the
total work isn't accidentally being held constant by an upstream
quota.

## Cross-cycle sample-set drift (a related rule)

If an experiment iterates across cycles and changes its sampling
strategy (e.g., cycle 2 samples the first 500K records; cycle 3
samples the first 5M records), the cross-cycle comparison is **not**
apples-to-apples. The paper must either:

1. Quote ONLY the latest-cycle number as the headline, or
2. Explicitly disclose the sample-set change ("cycle 3 expanded the
   sample window 10×; the comparison to cycle 2 is regime-specific").

This is the cycle-N analogue of the cycle-0 audit: same principle
(intended ≠ realised, here in the *what data are we measuring*
dimension), same mitigation (be explicit).

## Why this is its own playbook

The cycle-0 audit could be a single line in playbook 05 (hypothesis-
driven research), and the rule **is** referenced there. But it
deserves its own playbook because:

- It catches a class of bug that no other playbook catches.
- The seed session learned this rule the hard way (multiple
  recurrences), which justifies the prominence.
- It's cheap and mechanical — exactly the kind of rule that should be
  hard to forget. A dedicated playbook makes it easier to surface in
  agent prompts.

## Anti-patterns

- **Skipping the audit because "the knob obviously does what it says".**
  In the seed session, this assumption was wrong three times. The
  audit is cheap; skip nothing.
- **Documenting `intended=X` only.** If the audit table only lists
  intended values, it's not an audit — it's a recapitulation of the
  configuration. The realised column is the point.
- **Auditing once, then trusting forever.** When the harness changes,
  the realised value can drift even if the intended value didn't.
  Re-audit on every sweep, including re-runs of "the same" sweep
  with new code.
- **Interpreting drift as a finding.** If the cycle-0 audit reveals
  a 10× drift, the drift is **not** the finding — it's the *bug*.
  Fix it, then run the sweep that produces the finding.

---

*Origin: `data-pilot-research/AGENT_WORKTREE_PROTOCOL.md` rule 5f
("Cycle-0 knob-realisation audit rule", added 2026-05-17 after the
seed session's M3-M6 close-review).*
