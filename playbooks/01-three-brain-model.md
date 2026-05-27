# Playbook 01 — The Three-Brain Organisational Model

> The single most load-bearing structural decision in this methodology is to
> keep three logically-separate "brains" — and to be explicit about which
> brain a given action mutates.

## The model

| Brain | Lives in | Contains | Mutates when |
|---|---|---|---|
| **Product brain** | A sibling repo (the implementation that will *consume* findings, if one exists). | Production code, shipped features, release tags. | Only when shipping product code. Completely independent of the research repo. |
| **Research brain** | The research repo. | Experiments, raw data, paper drafts, milestone logs, learnings ledger, hypotheses. The *citable record*. | Only when an experiment runs, a paper is revised, or a milestone closes. Read-mostly during discussions. |
| **Discussion brain** | A single `DISCUSSIONS.md` file inside the research repo. | Plain-English Q&A between user and agent on what the research means, what's new, how to interpret it. Append-only. | Every discussion turn. |

## Why three brains, not one or two

The instinct of an unconstrained agent is to write everything into one
record: experiment results, draft prose, interpretations, conversation,
all interleaved. That instinct is wrong for four reasons.

### 1. Context hygiene

Research artifacts (milestone log, learnings ledger, paper draft) are *the
citable record*. They must not absorb running interpretations from a
chat — next turn the user may disagree with that interpretation and ask
for a revision. If revisions land directly in the research record, the
audit trail becomes noisy and hard to trust. If they accumulate in
`DISCUSSIONS.md`, you can correct freely without polluting findings.

### 2. Reversibility

A tagged snapshot of the research brain ("research-graduated-YYYY-MM-DD")
means "rewind to here" is a one-command operation. If a discussion finds
something wrong *and* you already wrote it into the research record, you
need a revert/explain dance. Better to argue in the discussion brain
until convinced, *then* consolidate explicitly.

### 3. Audit trail integrity

A reader six months from now wants to read either the **research story**
(papers + milestones + learnings) or the **discussion story**
(`DISCUSSIONS.md`), without interleaving. Both are valuable; they should
not be conflated. The three-brain split lets each tell its own coherent
narrative.

### 4. Cognitive load on the agent

The agent should not have to ask each turn "is this doing research, or
discussing?" The file destination decides:

- New experiment → branch + commit under `experiments/` + `ITERATIONS.md` + `MILESTONE_LOG.md`.
- Conversational understanding → `DISCUSSIONS.md`.
- Product idea spawned mid-discussion → separate workstream against the product brain.

The decision is structural, not judgment-based.

## The consolidation rule

Discussions move into the research brain **only on explicit user signal**.
Examples of such signals:

- "OK, consolidate this into the paper."
- "Add this finding to `notes/learnings.md`."
- "This deserves an experiment — run `EXP-00XX`."

Until the user gives an explicit signal, `DISCUSSIONS.md` accumulates and
the research brain stays put. The research brain *can* advance
independently if new experiments run, but those advances are separate
from any chat about existing findings.

### Consolidation procedure

When the user signals consolidate:

1. The original discussion entries stay in `DISCUSSIONS.md` *verbatim*
   (audit trail).
2. Write a single condensed entry summarising the convergent finding.
3. Propagate that entry into `notes/learnings.md` with a fresh L-ID, and
   into the paper draft if material.
4. The original discussion entries remain in `DISCUSSIONS.md` as the
   *rationale trail* for the consolidated entry.

This mirrors the discipline of the research brain itself: experiments
accumulate in `ITERATIONS.md`; only the load-bearing findings make it to
`notes/learnings.md`.

## Caveats

### Caveat 1 — Corrections found in discussion

When a discussion produces a *correction* (e.g., user spots a real bug
in a paper finding), that **is** a research-brain mutation. The
convention: do the mutation explicitly, in its own commit, and reference
the discussion entry that triggered it. Example commit message:
`paper rev-21 line 482 corrected per DISCUSSIONS.md Discussion 7`.

### Caveat 2 — Discussion brain is read-only on product brain

The discussion brain must not modify the product brain. If a discussion
produces a product idea ("implement feature Y in the viewer"), that
becomes a separate workstream against the product repo, *not* against
the research repo.

### Caveat 3 — Discussion file rollover

`DISCUSSIONS.md` is a single file. It will grow. Convention: when it
crosses ~30K words, spawn `DISCUSSIONS-YYYY-qN.md` (e.g.,
`DISCUSSIONS-2026-q3.md`). Append-only across all of them; never
rewrite or compact a prior file.

## Resumption protocol

If the discussion thread dies and resumes later:

1. Read the *tail* of `DISCUSSIONS.md` for context.
2. Verify the research brain is at the snapshot it should be at
   (`git rev-parse <tag>` against the tag the last entry referenced).
3. Optionally read the relevant paper section for grounding.
4. Continue the conversation; do not re-derive things.

## What about parallel agents?

When multiple sub-agents work concurrently inside the research brain
(e.g., one running an experiment, one revising a paper), they each get
their own git *worktree* on their own branch. The discussion brain
remains a single shared file; sub-agents do not write to it. Discussion
writes are owned by the orchestrator (the agent talking to the user).

See playbook **04-iterations-log** for the iteration-level discipline
and the seed session's `AGENT_WORKTREE_PROTOCOL.md` (referenced in case
studies) for the parallel-agent worktree pattern.

---

*Origin: `data-pilot-research/DISCUSSIONS.md` Discussion 3
(2026-05-18, "The 3-brain organisational model + research-brain
snapshot"). Generalised here to drop the specific product-brain path and
the specific snapshot tag.*
