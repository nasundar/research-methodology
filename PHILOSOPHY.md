# Philosophy — the research mindset

> Read this once before touching any other file in the repo. The playbooks
> are operational; this file is the worldview the playbooks operate inside.
> Without the worldview, the playbooks degenerate into a checklist.

## 1. Why this exists

Most research projects fail not at the experiment bench but at the
**discipline of asking the next question**. They produce a first-cut number,
declare it the finding, and move on. The findings end up brittle: a reviewer
asks "but why?" and the entire claim collapses, because the team measured
*what* without measuring *why*.

This repo encodes a small set of habits that, applied consistently, prevent
that collapse. The habits are domain-agnostic. They have been validated on
empirical systems research, but the discipline transfers to ML, empirical PL,
distributed-systems benchmarking, scientific computing, and any inquiry that
ships numbers attached to claims.

## 2. The four guiding principles

### 2.1 Ask *why* — and keep asking, 15 to 20 levels deep

A first-cut measurement is a starting point, not an answer. After every
result the researcher must ask, and *answer in writing*:

1. Why is the number this magnitude? What part of the system produces it?
2. What would make it better? What variants would move the number?
3. What did we get wrong? Where would a hostile reviewer poke?

These three questions are not bullets to tick. They are an iterative loop.
Question (1)'s answer is itself a claim; question (1) re-applies to that
claim. After 15–20 levels of "why?" you arrive at a *mechanism*, not a
correlation. Mechanisms are what graduate to papers; correlations are what
get retracted.

Playbook **10-going-deep** describes this loop in detail. The shorthand: do
not stop asking "why" until you can answer "and that mechanism is the same
mechanism we'd predict from first principles" — or you can explicitly state
"we predicted X from first principles; we measured Y; the gap is N, and the
gap is what we don't yet understand."

### 2.2 Treat synthetic results as a ceiling, not a finding

Any artificial setup — synthetic generator, mock workload, controlled
fixture — produces numbers that are *upper bounds* on what nature will let
you get. They are useful as ceilings ("our system cannot do better than this
even in ideal conditions") and as relative comparisons ("variant A beats
variant B under identical synthetic stress"), but they are **never** the
final claim.

The final claim must be anchored on the closest-to-reality dataset that
exists, and any synthetic result must be validated against that reality with
side-by-side moments (cardinality, attribute count, body length, whatever
the domain-relevant distributions are). When the synthetic and real
distributions diverge by more than ~2×, the synthetic generator is
mis-calibrated and the experiment must be re-run or the claim re-scoped.

### 2.3 Treat every result with "but why?" and "can we do better?"

A surprising number is data; the mechanism behind it is the research. A
non-surprising number is also data; the mechanism behind it is *also* the
research — sometimes the most valuable, because it confirms or refutes a
prediction.

Every result deserves two follow-up moves:

- **Mechanism** — write down, in plain English, what physical or logical
  process produced this magnitude. Predict what would happen if you varied
  one input. Run the variation. Confirm or refute.
- **Improvement attempt** — list 2–4 candidate variants that could move
  the number in the desired direction. Predict each variant's effect
  (direction and magnitude). Pick the strongest-prediction variant. Run it.

If both moves return "yes, the mechanism is what we predicted and no
improvement is available," that's a *finding*. It's also rare. Most of the
time at least one of the two moves uncovers something the team didn't see
on the first cut.

### 2.4 Productive falsification beats defensive confirmation

When a reviewer proposes a hypothesis that *competes* with your claim, the
amateur researcher designs an experiment that defends the current claim.
The disciplined researcher designs an experiment that **tests the
reviewer's hypothesis on its own terms**. There are two outcomes:

- **The reviewer is right.** You update the paper. The science has
  advanced. This is fine; sometimes it's the whole point.
- **The reviewer is wrong.** The refutation is rarely sterile. Because
  you designed the experiment around the reviewer's claim — with the
  axes and structure the claim implied — the refutation typically reveals
  the *actual* mechanism, which had been hidden behind the reviewer's
  proposed one. This is **productive falsification**, and it has produced
  some of the strongest findings in the seed session that originated this
  repo. See playbook **09-productive-falsification** for the pattern.

A corollary: pre-register reviewer hypotheses *as your own predictions*
(with bands and falsification criteria) before running the experiment.
Then the commit graph cryptographically demonstrates prediction-before-
measurement, which closes the HARKing-after-the-fact failure mode at the
git layer.

## 3. The discipline that follows from the principles

The four principles above generate a small number of operational habits:

| Habit | Where it lives | Why |
|---|---|---|
| Three-brain organisation | Playbook 01 | So discussion turns don't pollute the research record. |
| Append-only logs | Playbooks 02, 03, 04 | So the audit trail survives 6 months of re-reading. |
| `HYPOTHESIS.md` before measurement | Playbook 05 | Prevents HARKing; enables productive falsification. |
| V1–V6 verification | Playbook 06 | Breaks the fix-one-bug-introduce-another anti-pattern. |
| Standing reviewer pair | Playbook 08 | Two reviewers catch disjoint bug classes. |
| Cycle-0 knob audit | Playbook 11 | Catches the "I think my knob does X but it actually does Y" methodology bug *before* it becomes a finding. |
| Reviewer-bait flags | Playbook 12 | Self-disclosed honesty is cheaper than reviewer-found embarrassment. |
| Mechanism, then iteration | Playbook 10 | A first-cut result is the start, not the end. |

None of these habits is novel in isolation. The contribution of this repo
is the *combination* and the discovery that, run together, they produce
research that survives close review with significantly less drama than
research that runs any subset.

## 4. What graduates and what doesn't

A finding **graduates** to the paper when:

1. The mechanism is explained, not just measured.
2. The result has been measured on the closest-to-reality dataset available.
3. The result has been measured with `n ≥ 3` seeds (or carries a paired-CV
   from a same-harness prior measurement on the same configuration).
4. The result has been verified by the reviewer pair, both verdicts at
   `ACCEPT` or `MINOR-with-disclosure`.
5. The result has survived at least one explicit attempt to falsify it
   (variant sweep, mechanism falsifier, or cross-axis stress).

A finding does **not graduate** if it leans on a single seed, lacks a
mechanism, has only been measured on synthetic data, or has not been
challenged. It can still be a paragraph in the supplement under "open
questions" or "future work" — but the headline-load-bearing claims have to
clear all five bars.

## 5. The "but why?" loop is the entire job

If you remember nothing else from this file: **the job is to keep asking
"but why?" until either you've hit first principles, or you've drawn a
small explicit box around the gap between measurement and prediction**.

Everything in this repo — the logs, the templates, the verification
protocol, the reviewer pair, the cycle-0 audit — exists to make that loop
cheap enough that you actually run it instead of cutting it short.

## 6. How this repo evolves

This repo is itself a research artifact. Each playbook ends with an
"Origin" footer pointing to the source material that informed it. When you
discover a new failure mode in your own work that the existing playbooks
do not cover, the right move is to:

1. Name the failure mode (give it a label).
2. Write a short case study under `case-studies/`.
3. Either add a new playbook or extend an existing one.
4. Update the relevant template if the failure mode is preventable at
   template time.

The V1–V6 protocol in playbook 06 grew this way: it started as V1–V5,
extended to V1–V6 after one specific cross-section failure escaped, and
will likely grow to V1–V7 in any sufficiently long-running project. The
discipline is **monotone-extensible**: new layers are added, old layers
are never removed.

---

*Origin: synthesised from `data-pilot-research/paper/methodology-workshop-draft.md`
§§ 1, 8.6, 9 and from `data-pilot-research/RESEARCH_DEPTH_PROTOCOL.md`.*
