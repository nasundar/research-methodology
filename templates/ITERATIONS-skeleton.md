# ITERATIONS — `<project-name>`

> **Append-only log of every hypothesis tested.** One block per
> experiment.
> Format: `EXP-NNNN` | date | hypothesis (1 line) | result | reviewer
> verdict | decision (advance / re-roll / kill).
> Full write-ups live in `experiments/NNNN-<slug>/RESULTS.md`.

---

## EXP-0001 — <one-line title>

- **Date:** YYYY-MM-DD
- **Hypothesis (1 line):** <the H you committed in HYPOTHESIS.md>
- **Result:** <PASS / FAIL / SURPRISE / REFUTED — one paragraph>.
- **Surprise finding (optional):** <anything unexpected>
- **Mechanism (one paragraph):** (full version in `MECHANISM.md`)
- **Reviewer verdict:** <ACCEPT / MINOR / MAJOR / REJECT> or
  "deferred — bundling with EXP-NNNN+k at next milestone".
- **Cross-cycle reconciliation (optional):** if this changes the
  interpretation of a prior EXP.
- **Decision:** advance / re-roll / kill / queue follow-up.
- **Hostile-reviewer questions (optional):** see RESULTS § N.
- **Files:** <list>.

---

## EXP-0002 — <one-line title>

- **Date:** YYYY-MM-DD
- **Hypothesis (1 line):** ...
- **Result:** ...
- **Reviewer verdict:** ...
- **Decision:** ...
- **Files:** ...

---

*(continue, one section per experiment, in chronological order)*

---

*This skeleton lives in `templates/ITERATIONS-skeleton.md`.
Convention from playbook 04.*
