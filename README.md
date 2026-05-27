# research-methodology

> A domain-agnostic, PhD-level research playbook. Drop this repo into a fresh
> agent session as context, and it will know how to run hypothesis-driven
> research, log it correctly, verify it against itself, and graduate papers
> through a standing reviewer pair without shipping bugs.

## What this repo is

This is **not** a project. It is a portable **methodology** — the discipline
and habits a research agent (human + AI) should adopt when starting any new
research investigation, in any domain.

It was extracted from a 23-day agent-driven systems-research session in which
two papers graduated to UNANIMOUS ACCEPT through 51 experiments, 30 paper
revisions, and 19 reviewer cycles. The specifics of that session were
data-pipeline and observability storage; the discipline is general.

## What you get

| File / dir | Purpose |
|---|---|
| `PHILOSOPHY.md` | The "why & how" research mindset. Read first. |
| `HOW-TO-INVOKE.md` | Step-by-step for an agent in a fresh session. |
| `playbooks/` | 13 numbered playbooks — each readable in 5 minutes. |
| `templates/` | Copy-paste skeletons for logs, experiments, hypotheses, papers. |
| `case-studies/` | Worked examples extracted from the seed session. Read for intuition. |
| `skill/` | Copilot-CLI skill description, agent instructions, invocation examples. |

## The 13 playbooks at a glance

| # | Playbook | TL;DR |
|---|---|---|
| 01 | Three-brain model | Product / Research / Discussion brains, with explicit consolidation rule. |
| 02 | Milestone log | Append-only chronological "what was done". |
| 03 | Discussions log | Append-only human↔agent Q&A; never mutates research brain. |
| 04 | Iterations log | Append-only experiment register — one entry per `EXP-NNNN`. |
| 05 | Hypothesis-driven research | `HYPOTHESIS.md` before measurement, every time. |
| 06 | V1–V6 verification | Six grep/recompute audits the agent runs before every commit. |
| 07 | L1.21 anti-pattern | Fix-one-bug-introduce-another, and how to recognise + break it. |
| 08 | Standing reviewer pair | Two parallel reviewers from disjoint model families. |
| 09 | Productive falsification | Refuting a reviewer's hypothesis is sometimes the win. |
| 10 | Going deep | 15–20 levels of "why?", not just "how?". |
| 11 | Cycle-0 knob audit | Do your knobs actually do what you think? Check before measuring. |
| 12 | Reviewer-bait flags | Self-disclose your honest weaknesses in every revision. |
| 13 | Paper graduation | Verdict-trajectory model; when is a paper done? |

## How to use this repo

**If you're a human** starting a new research project, read in this order:
`PHILOSOPHY.md` → `HOW-TO-INVOKE.md` → playbooks 01–05 → playbooks 06–13 as
they become relevant.

**If you're an AI agent** loaded into a fresh session and asked to start
research, read `skill/agent-instructions.md` first, then follow the protocol
in `HOW-TO-INVOKE.md`.

## Status

Initial scaffolding, v0.1. Open to corrections and additions — see
`PHILOSOPHY.md` § "How this repo evolves".
