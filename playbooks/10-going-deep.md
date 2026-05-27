# Playbook 10 — Going Deep ("Ask Why, Not Just How")

> Each experiment is the **start** of an investigation, not the end. A
> first-cut number is data; the mechanism behind it is the research. Keep
> asking "but why?" — 15 to 20 levels deep — until you arrive at either
> first principles or an explicit, bounded statement of what you do not
> yet understand.

## The principle

A first-cut measurement is one of three things, and you must figure out
which:

1. **Confirmation of a predicted mechanism.** Boring but valuable. The
   prediction in `HYPOTHESIS.md` matched. You move on.
2. **A surprise** (positive or negative). The result is outside the
   pre-registered band. *This is the start of the research.* You owe
   it a `MECHANISM.md` and at least one follow-up cycle.
3. **A methodology bug.** The result is impossible or inconsistent
   with adjacent measurements. You owe a cycle-0 knob audit
   (playbook 11) before you owe anything else.

Most first-cut measurements are 1 or 2. The job is to distinguish them
and then to follow case 2 all the way down.

## The "but why?" loop

After every experiment, the researcher answers in writing:

1. **Why is this number what it is?** Mechanism — what part of the
   system produced this magnitude, in what proportion, under what
   assumptions?
2. **What would make it better?** Variant designs, alternative
   encodings, different operating points, profile-specific paths.
   List 2–4 candidate improvements; predict each's direction and
   magnitude; pick the strongest-prediction one.
3. **What did we get wrong?** Where would a hostile reviewer poke?
   What assumptions are we taking for granted that could be
   invalidated?

Each answer is itself a claim, and (1) re-applies to it. Iterate.

The deliverable for every experiment is therefore three artifacts:

- `experiments/NNNN-<slug>/RESULTS.md` — the measurement.
- `experiments/NNNN-<slug>/MECHANISM.md` — the why-and-how analysis
  (this is where the "but why?" loop lives).
- A follow-up dispatched (or a recorded decision in `MECHANISM.md`
  § Iteration Plan explaining why no follow-up is needed).

## How deep is "deep enough"?

The seed session settled on this rule of thumb:

- **Don't stop until** either:
  - You can explain the magnitude from first principles, AND a
    prediction from those principles matches the measurement to
    within the pre-registered band; OR
  - You can draw an explicit, bounded box around the gap between
    measurement and first-principles prediction. ("Predicted X from
    mechanism; measured Y; gap is Z. Z is what we don't yet
    understand. Possible explanations: ... Future work.")

Empirically this typically takes **15–20 levels of "why?"** for a
load-bearing finding. Not all 15 are large reasoning steps; many are
small confirmations ("at this level the mechanism is just dictionary
encoding"; "at this level the mechanism is the dictionary's per-row-
group reset behaviour"; "at this level the mechanism is the writer's
spill policy when the dict cap is exceeded"; …). Each level eliminates
one possible explanation.

Most amateur research stops at level 2 or 3. Most disciplined research
stops at level 8–10 (good enough for a paper). Going to 15–20 is what
distinguishes a paper that survives close review from a paper that
gets retracted on a single hostile question.

## Iteration patterns

The seed session catalogued five iteration patterns that recur. They
are reusable templates for *what to do next* after a first cut:

### Pattern A — Variant sweep

After a single-point result, run 2–4 nearby variants and report the
curve. *Example:* if a parameter `R` matters and `R=128K` gave the
first result, also run `R ∈ {32K, 512K, 1M, 2M}` and plot.

### Pattern B — Mechanism falsification

After a mechanism claim, run an experiment **designed to disprove the
mechanism**. *Example:* if you claim "the storage cost is dominated by
metadata", a mean-body-tokens sweep should make body dominate; if the
ratio stays the same, the metadata story is wrong.

### Pattern C — Adjacent-systems probe

After a finding on one component, ask "does this generalise to the
next system over?" *Example:* a finding on a 3-input configuration
might generalise to a 5-input configuration — or it might break.
Either result is informative.

### Pattern D — Reviewer-bait

After a finding, simulate what a hostile reviewer would ask, and
pre-answer. *Example:* "How does this hold under cold cache?" → run
the cold-cache variant. "What about at 10× the scale?" → run the
next decade.

### Pattern E — Cross-axis stress

After a finding on axis A, vary axis B and report the joint behaviour.
Often the original finding is conditional on a specific value of B
that nobody varied.

## Per-agent iteration prompt template

When dispatching an experimental sub-agent, include this paragraph
verbatim:

> When your first-cut measurement lands, **do not stop**. Write a
> `MECHANISM.md` in your experiment directory that answers (a) why is
> the number this magnitude, (b) what 2–4 candidate improvements would
> move it further, predicted direction + magnitude, (c) what's the
> strongest falsifier of your mechanism claim, and (d) which iteration
> you recommend running next. Then **run that iteration** (or document
> why it should be a separate worktree agent's job and create the
> follow-up todo). Commit the iteration's results alongside the first
> cut. Your reply to the orchestrator should describe **both cycles**,
> not just the first measurement. Headline numbers without mechanism
> are not paper-grade research.

## When to stop iterating

Two stopping rules:

1. The marginal improvement from one more cycle is below ~5% of the
   headline number, **and** the mechanism explanation is consistent
   with first-principles prediction.
2. The paper's "remaining open questions" section is large enough to
   swallow the further exploration as out-of-scope-for-this-paper.

Either is acceptable. What's not acceptable is stopping because the
team is tired or because the deadline pressure is rising. If the
result is paper-headline-load-bearing, the discipline is to iterate
until one of the two stopping rules fires.

## Iteration cycle counting

The convention: a milestone is **not closed** until each load-bearing
experiment has gone through at least **two** iteration cycles:

- Cycle 1 = first cut + `MECHANISM.md`
- Cycle 2 = at least one improvement attempt with its own results and
  mechanism

For paper-headline-load-bearing cells, the floor is **three or more**
cycles. The depth-cycle count is tracked in `MILESTONE_LOG.md` (an
explicit column) so the reader can see at a glance which experiments
are one-cycle (exploratory) vs N-cycle (paper-grade).

## Don't optimise for completion velocity

Closing 6 in-flight todos in 20 minutes is **worse** than closing 3
todos with 2 iterations each in 40 minutes. The latter is research;
the former is checklist execution.

When in doubt, slow down and iterate. Going deep is the entire
research function. Going broad is project management. Don't confuse
the two.

## Orchestrator's role

After every sub-agent completes:

1. Read the sub-agent's `MECHANISM.md` (require it; absent = blocking).
2. Pick the strongest candidate improvement from the sub-agent's
   predictions.
3. Dispatch a follow-up sub-agent in a new worktree to run it.
4. Repeat until a stopping rule fires.

Reviewers (the standing pair, playbook 08) run on the **iteration
result**, not the first cut, whenever feasible.

## Anti-patterns

- **First-cut shipping.** Reporting the first number as the finding,
  without `MECHANISM.md`, without a falsifier. Almost always
  retracts under review.
- **Single-pattern iteration.** Always running variant sweeps
  (pattern A) and never running mechanism falsifiers (pattern B).
  Variant sweeps tell you the curve; falsifiers tell you whether
  your story explains the curve.
- **Stopping at "approximately correct".** A mechanism story that
  predicts magnitude to 1.5× is *not* paper-grade. Paper-grade is
  prediction-to-band (e.g., ±15%) at the cells the paper claims to
  cover.
- **Counting cycles you don't run.** A milestone with "we'd run a
  cross-axis stress but it's out of scope" entries in every
  load-bearing experiment is a milestone with no actual
  cross-axis-stress evidence. Sometimes that's fine; document it
  honestly.

---

*Origin: `data-pilot-research/RESEARCH_DEPTH_PROTOCOL.md` (2026-05-17
user directive) and the seed session's per-experiment `MECHANISM.md`
discipline.*
