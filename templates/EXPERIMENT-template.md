# EXP-NNNN — `<one-line title>`

> **Skeleton for `experiments/NNNN-<slug>/` directory.** Copy this file
> alongside the three required artifacts:
>
> - `HYPOTHESIS.md` (see `templates/HYPOTHESIS-template.md`)
> - `RESULTS.md` (this file template lives here)
> - `MECHANISM.md` (see § "MECHANISM.md outline" below)
>
> Plus `data/` (raw outputs, gitignored if large) and `charts/`.

---

## experiments/NNNN-<slug>/RESULTS.md outline

```markdown
# EXP-NNNN RESULTS — <title>

## TL;DR

<one paragraph: what was measured, what was found, PASS/FAIL on each
pre-registered Hi>.

## Setup

- **Hypothesis file:** `HYPOTHESIS.md` committed at `<short-sha>`.
- **Configuration:** (versions, seeds, hardware, knobs).
- **Cycle-0 audit result:** (link to `HYPOTHESIS.md § 0` table).
- **Sample-set:** (n seeds, sample-window, any cross-cycle drift
  disclosure per playbook 11).

## Measurement

(Tables, plots, numbers. Each headline number cites its source
file under `data/`.)

## Hypothesis scorecard

| Hi | Predicted | Measured | Verdict |
|---|---|---|---|
| H1 | <band> | <number ± CI> | PASS / FAIL / REFUTED |
| H2 | ... | ... | ... |

## Cross-cycle reconciliation (if applicable)

(If this experiment's result changes the interpretation of a prior
result, state the reconciliation explicitly.)

## Decision

- **Advance / re-roll / kill:** (decision)
- **Follow-ups queued:** (list of EXP-NNNN+k or future-work items)

## Files

- (list of new/changed code, notes, paper sections, charts)
```

---

## experiments/NNNN-<slug>/MECHANISM.md outline

```markdown
# EXP-NNNN MECHANISM — <title>

## § 1 Why is this number what it is?

<Mechanism story. What part of the system produced this magnitude,
in what proportion, under what assumptions? Go ≥ 5 levels deep before
stopping (playbook 10).>

## § 2 Candidate improvements

| Variant | Predicted direction | Predicted magnitude | Cost to run |
|---|---|---|---|
| (a) <description> | <+/-> | <%> | <S/M/L> |
| (b) ... | ... | ... | ... |

**Pick:** <strongest-prediction variant>, dispatched as EXP-NNNN+k.

## § 3 Strongest falsifier

<What is the strongest single experiment that would refute the
mechanism claim in § 1? Specify the design.>

## § 4 Hostile-reviewer questions

(1) ...
(2) ...
(N) ...

## § 5 Iteration plan

<which iteration runs next; or "no follow-up needed because (reason)".>
```

---

## Directory checklist

For every `EXP-NNNN`, before declaring the experiment closed:

- [ ] `HYPOTHESIS.md` committed in a commit that *precedes* the result
      commit (git-temporal rule, playbook 05).
- [ ] `HYPOTHESIS.md § 0` cycle-0 audit completed; any drift > 5% is
      resolved (playbook 11).
- [ ] `RESULTS.md` written with hypothesis scorecard.
- [ ] `MECHANISM.md` written with at least the 5 sections above.
- [ ] At least one follow-up dispatched, OR `MECHANISM.md § 5`
      documents why no follow-up is needed.
- [ ] `ITERATIONS.md` updated with one entry for this EXP.
- [ ] Any durable L-ID lessons added to `notes/learnings.md`.
- [ ] If milestone-closing: `MILESTONE_LOG.md` entry appended.

---

*Origin: the seed session's `experiments/NNNN-*/` directory shape; the
`RESULTS.md` + `MECHANISM.md` + `HYPOTHESIS.md` discipline from
`data-pilot-research/RESEARCH_DEPTH_PROTOCOL.md`.*
