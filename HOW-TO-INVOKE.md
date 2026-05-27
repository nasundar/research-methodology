# How to invoke this playbook from a fresh agent session

> You are an AI agent. The user has loaded this repo into your context and
> said something like *"I want to do PhD-level research on X"* or *"help me
> set up a research project"* or *"use the research methodology"*. This file
> tells you exactly what to do.

## Step 1 — Confirm you've loaded the repo

Before doing anything else, verify these files exist in your context:

- `PHILOSOPHY.md` (you should have read it before responding to the user)
- `playbooks/01-three-brain-model.md` through `playbooks/13-paper-graduation.md`
- `templates/` (six skeleton files)
- `skill/agent-instructions.md`

If any are missing, ask the user to confirm the load. Do **not** improvise
the missing playbook content from memory — every playbook has an `Origin`
footer for a reason.

## Step 2 — Take the research-project intake

Ask the user the following four questions, one at a time (or in a single
short message if the user has already given you enough context):

1. **What's the research question?** A single sentence. If the user can't
   compress it to a sentence, the project isn't ready — help them
   sharpen it.
2. **What's the falsifiable counter-hypothesis?** "The null hypothesis"
   in domain-neutral language. If there isn't one, the research question
   is too vague.
3. **What is the closest-to-reality dataset / environment / corpus that
   exists for this question?** Even if you also need synthetic. Synthetic
   alone is a ceiling, not a finding (PHILOSOPHY.md § 2.2).
4. **What's the venue / output target?** Conference paper, internal
   report, blog post, decision-support document. This calibrates the
   discipline level. A blog post does not need V1–V6 verification; a
   conference paper does.

## Step 3 — Set up the three brains

Apply playbook **01-three-brain-model**:

- **Product brain** — the implementation that may consume the findings.
  If it exists already, point at it and confirm it's *not* the same repo
  as the research.
- **Research brain** — the repo that will hold experiments, papers, logs.
  Either create it now or confirm the path.
- **Discussion brain** — a single `DISCUSSIONS.md` file in the research
  repo, append-only.

Each brain has its own file and rules. Do not let discussion turns mutate
the research brain; do not let research commits land without a milestone
log entry; do not let product changes leak into the research repo.

## Step 4 — Initialise the research repo

In the research brain (the repo or directory the user designates), create
these files using the corresponding templates from `templates/`:

| File | Template |
|---|---|
| `README.md` | (handwritten — high-level project shape; cite this repo as methodology source) |
| `PLAN.md` | (handwritten — the master plan; sketch milestones M0..MN) |
| `MILESTONE_LOG.md` | `templates/MILESTONE_LOG-skeleton.md` |
| `DISCUSSIONS.md` | `templates/DISCUSSIONS-skeleton.md` |
| `ITERATIONS.md` | `templates/ITERATIONS-skeleton.md` |
| `notes/learnings.md` | (start empty with a heading — populate as findings emerge) |
| `experiments/0001-<slug>/HYPOTHESIS.md` | `templates/HYPOTHESIS-template.md` |
| `paper/skeleton.md` | `templates/paper-skeleton.md` (use only when a paper is in scope) |

## Step 5 — Adopt the discipline before running anything

Confirm with the user that the following are agreed:

- Every experiment has a `HYPOTHESIS.md` committed **before** the
  measurement commit (playbook 05). The git graph is the audit trail.
- Every commit that touches a paper draft runs the V1–V6 verification
  protocol (playbook 06). The agent's revision prompt **must** include
  the six checks.
- Reviews are run by a standing pair (playbook 08): two reviewers from
  disjoint model families, same prompt, parallel dispatch. Default
  pair: `claude-opus-4.7-xhigh` + `gpt-5.5`, or whatever cross-family
  pair is current.
- Every parameter sweep starts with a cycle-0 knob-realisation audit
  (playbook 11).

If the venue is a blog post or internal note, V1–V6 and the reviewer
pair are optional but the cycle-0 audit, hypothesis pre-registration,
and the append-only logs are not.

## Step 6 — Run the first cycle

For the first experiment (`EXP-0001` by convention):

1. Write `experiments/0001-<slug>/HYPOTHESIS.md` from the template.
2. Commit the prediction-only file. **Stop here.** Do not measure yet.
3. Run the measurement. Land raw outputs under `experiments/0001-<slug>/data/`.
4. Write `experiments/0001-<slug>/RESULTS.md` (paper-grade prose) and
   `experiments/0001-<slug>/MECHANISM.md` (why-and-how analysis,
   per playbook 10).
5. Update `notes/learnings.md` with any durable lesson (L-IDs by topic).
6. Append a single line to `ITERATIONS.md` (one entry per EXP).
7. Append a section to `MILESTONE_LOG.md` if a milestone closed.

That is one full cycle. Repeat for every experiment.

## Step 7 — When a paper is ready

When a draft is in shape, dispatch the standing reviewer pair. Read
playbook **08-standing-reviewer-pair** for the dispatch convention and
playbook **13-paper-graduation** for the verdict-trajectory model.

Expect to take 5–10 reviewer cycles to graduate. Each cycle, run V1–V6
against the revision *before* sending to the reviewers (playbook 06).
Treat any new bug class the reviewers introduce as a candidate for V7
extension (playbook 06 § "Monotone extensibility").

## Step 8 — Reset between research tasks

If the user pivots to a new research question, do **not** carry over the
old research brain. Create a new one. Carry over only this repo (the
methodology) and the agent-instructions in `skill/agent-instructions.md`.

This keeps the three-brain hygiene intact and prevents stale findings
from one project from biasing the framing of the next.

---

## Common mistakes to avoid

- **Skipping the cycle-0 audit because "the knob obviously does what it
  says."** It often doesn't. See playbook 11 and the seed session's
  13× phantom (playbook 11 case study).
- **Running V1–V6 only on revisions, not on the first paper draft.**
  The protocol is per-commit, not per-revision.
- **Letting the discussion brain seep into the research record.** If a
  chat produces a correction, do the correction explicitly, in its own
  commit, with a reference back to the discussion entry (playbook 01).
- **Single-reviewer dispatch.** A single reviewer will graduate a paper
  with a load-bearing contradiction. Always two.
- **Treating reviewer hypotheses as adversarial.** Treat them as
  pre-registrations. See playbook 09.

---

*Origin: synthesised from `data-pilot-research/AGENT_WORKTREE_PROTOCOL.md`,
`data-pilot-research/RESEARCH_DEPTH_PROTOCOL.md`, and
`data-pilot-research/paper/methodology-workshop-draft.md` §§ 2, 4.*
