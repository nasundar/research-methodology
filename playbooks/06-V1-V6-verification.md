# Playbook 06 — The V1–V6 Verification Protocol

> Six layered audits an agent runs before every paper-revision commit.
> Each step targets a bug class that the prior step misses. The protocol
> was discovered as a side-effect of breaking the L1.21 fix-one-bug-
> introduce-another anti-pattern (playbook 07).

## The pattern V1–V6 is solving

When an agent revises a paper draft to address reviewer findings, it
reliably closes the prior bugs while introducing new ones — at
progressively smaller scales, but at every cycle. The pattern is
*deterministic*: it recurred across 10 consecutive revisions of two
unrelated papers in the seed session, and it does not converge on its
own.

V1–V6 is the layered audit that breaks it. Each step exists because a
specific bug class escaped the previous steps.

## The six checks

| Step | Targets | Tool |
|---|---|---|
| **V1** | Literal-leftover wrong claim | `git grep` for the bug's literal string |
| **V2** | Surviving counter-claim | `git grep` for opposing/replaced claims |
| **V3** | Cross-section inconsistency | Pick out load-bearing constants; verify all sites use them consistently |
| **V4** | Wrong numerical recomputation (including denominator convention) | Recompute against source artifacts; check denominator convention |
| **V5** | Undisclosed scope drift | `wc -w` and header count vs target |
| **V6** | Stale code reference / lint failure | Lint and test the code the prose references |

Each step is run **per commit**, not per revision. Mandatory before
push.

### V1 — Surviving-wrong-claim grep

For each closed reviewer finding, emit the **literal string** the bug
appeared as and grep the paper for surviving occurrences. Example:

```bash
# Reviewer said the value should be 147, not 50:
git grep -nE '50K?\b|50,000|3× reduction|3-fold' paper/
```

V1 catches the **literal-leftover** bug class. It fails when the same
claim appears in a near-synonym you didn't enumerate. Mitigation:
accumulate V1 templates across the project; review the template list
before every commit.

### V2 — Counter-claim grep

V1 finds surviving wrong claims; V2 finds surviving **counter-claims** —
places where the paper still asserts the *opposite* of what the new
finding says. Example: after a finding promoted "piecewise" structure
over a prior "linear" prediction, V2 greps for:

```bash
git grep -nE 'linear K|≈ 50.+per cohort|linear scaling' paper/
```

V2 fails when the counter-claim is paraphrased in a way the grep
doesn't catch. Mitigation: same as V1 — accumulate templates over
time.

### V3 — Cross-section consistency

V1–V2 are per-section; V3 is cross-section. For every load-bearing
**constant, formula, or attribution** introduced or modified in the
revision, V3:

1. Picks the constant out explicitly.
2. Greps the paper for *all* sites that depend on it.
3. Verifies each site uses the constant consistently with the
   paper's stated formula.

Example:

```bash
for const in '175.3' '87.9' '3.242' '332' '182' '3.30'; do
  echo "=== $const ==="
  git grep -n -- "$const" paper/
done
```

The operational difficulty of V3 is that it requires the agent to
**enumerate the load-bearing constants explicitly** before commit. A
useful artifact: pre-commit a dependency-graph note for every
load-bearing constant — *which other sites of the paper would change if
this constant changed?* V3 runs against that artifact, not against the
agent's ad-hoc enumeration.

### V4 — Numerical recomputation against source data

For every numerical claim in the revision's diff, recompute the
number from the source artifact (RESULTS.md, MECHANISM.md, the
experiment's CSV/JSON).

**Extended with denominator audit.** For every residual or normalised
quantity, compute the value under **both denominator conventions**
(e.g., `residual / predicted` vs `residual / measured`) and confirm
the convention used matches the paper's stated convention elsewhere.

The denominator failure mode is particularly insidious because the
*direction* of the residual is preserved (−25% under one convention,
−36% under the other); only the magnitude shifts. A directional
sanity check passes, but the reviewer catches the magnitude.

### V5 — Word count + header count (budget audit)

The revision-agent prompt fixes a target word count (e.g., "stretch
target 16,800; hard cap 17,200"). V5 reports actual word count and
delta:

```bash
wc -w paper/<rev>.md
git grep -cE '^#{1,6} ' paper/<rev>.md
```

V5 catches **scope creep**: when the agent expanded sections beyond
what the dispatch asked for. Disclosed overshoots are fine; undisclosed
overshoots indicate the agent has gone off-prompt. V5 is cheap and
high-signal as a *prompt-adherence* check.

### V6 — Build / lint sanity on adjacent code

Run the project's lint + test gates against any code the revision
touched (typically `RESULTS.md` cross-references, experiment harness
test cases, or `HYPOTHESIS.md` constants):

```bash
cargo clippy --workspace -- -D warnings
cargo test --workspace --release --features <relevant>
```

(Replace with the project's actual gate.) V6 catches the case where
the revision agent edits prose that references a function name or
env-var that does not exist in the code. The build fails before the
reviewer has to find it.

## The layering principle

| Step | Catches what the prior step misses |
|---|---|
| V1 | (Base case) |
| V2 | Counter-claims that survived V1's literal grep |
| V3 | Cross-section drift hidden inside narrowly-correct sections (V1–V2 pass per-section) |
| V4 | Numerical errors that V1–V3 can't detect because the wrong number isn't a literal leftover |
| V5 | Scope drift that V1–V4 can't see because everything in scope is correct |
| V6 | Code/prose desync that V1–V5 can't see because prose is internally consistent |

The protocol is **monotone-extensible**: each new failure mode caught
by reviewers adds one new step (or extends an existing one) and is
then prepended to all future dispatch prompts. V3 was added after a
cross-section failure escaped V1–V2; V4 was extended after a
denominator failure escaped V3. V7 is anticipated as **narrative
consistency** (abstract claim vs body claim drift), but had not yet
been formalised at the close of the seed session.

## When to skip a step

Never. The cost of running V1–V6 is small (a few minutes of grep +
one numerical recheck per claim). The cost of shipping a paper with a
load-bearing arithmetic error is large. The asymmetry is the entire
argument.

If a step is genuinely not applicable (e.g., V6 on a pure-prose paper
that touches no code), document the skip explicitly in the commit
message: `V6: not applicable; no code referenced in this revision.`

## Operational integration

The revision-agent prompt must contain, at the end:

```
Before committing, run V1–V6 per playbook 06. Emit the output of each
step in your reply. Do not commit if any step reports a finding that
contradicts the revision's stated claims.
```

The orchestrator (the agent talking to the user) accumulates the V1
and V2 grep templates across the project and prepends them to every
dispatch.

## What V1–V6 does NOT do

V1–V6 closes **execution-class** bugs. It does not close:

- **Mechanism errors** (the paper's claimed mechanism is wrong).
  Caught by close reviewers (playbook 08) and by reviewer-bait flags
  (playbook 12).
- **Scope errors** (the paper's claim is too general).
  Caught by hostile-reviewer simulation in `MECHANISM.md` and by
  pre-registration bands (playbook 05).
- **Cosmetic errors** (typos, formatting). Not load-bearing; the
  reviewer pair surfaces them as A3-class and the team chooses
  whether to drive them to zero.

V1–V6 is *necessary but not sufficient*. The other playbooks close
what it leaves open.

## Empirical evidence the protocol works

In the seed session, V1–V6 was added incrementally:

- Before any V protocol: 5 consecutive revisions across two papers
  introduced new A1-class (blocking) bugs.
- V1–V5 active: one revision passed with one microscopic A3-class
  leftover (first ACCEPT in 7 cycles).
- V1–V5 + V3 extended: passed.
- V1–V6 with extended V4 (denominator): UNANIMOUS ACCEPT, zero new
  bugs introduced.

This is not a sample large enough to claim a general law; it is large
enough to establish that the protocol *can* break the L1.21 pattern.

---

*Origin: `data-pilot-research/paper/methodology-workshop-draft.md` § 4
and Appendix B. The case studies for V3 (`L1.22`) and V4-extended
(`L1.23`) appear in `case-studies/L1.21-case-2.md`.*
