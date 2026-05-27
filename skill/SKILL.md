---
name: research-methodology
description: |
  A PhD-level research methodology skill. Use when the user asks to set up
  a research project, do hypothesis-driven research, run a research session,
  or apply this methodology to a new domain. Triggers include phrases like
  "I want to do PhD-level research on X", "help me set up a research
  project", "use the research methodology", "apply the V1-V6 protocol",
  "set up a hypothesis-driven investigation". Use when the user mentions
  the L1.21 anti-pattern, the three-brain model, productive falsification,
  the standing reviewer pair, or any other named element of this playbook.
---

# research-methodology skill

> Skill-style entry point for an AI agent loaded into a fresh session that
> needs to invoke this methodology playbook.

## What this skill does

When invoked, this skill points the agent at the 13 playbooks, 6 templates,
and 4 case studies in this repository. It instructs the agent to:

1. Read `PHILOSOPHY.md` (the worldview).
2. Read `HOW-TO-INVOKE.md` (the step-by-step).
3. Apply the playbooks in the order the project requires.
4. Use the templates to scaffold the user's new research repo.
5. Reference the case studies when calibrating expectations about review
   cycles, L1.21 cascades, and productive falsification.

## When to invoke

Invoke this skill when the user:

- Asks for **PhD-level** or **publication-grade** research help on any
  topic in any domain.
- Mentions setting up a **research project**, a **research repo**, or a
  **hypothesis-driven** investigation.
- Mentions any **named element** of the methodology by name: the
  three-brain model, the V1–V6 verification protocol, the L1.21
  anti-pattern, the standing reviewer pair, productive falsification,
  the cycle-0 knob audit, reviewer-bait flags, paper graduation.
- Asks how to **review** a paper draft or how to **dispatch reviewers**.
- Asks how to **structure logs** (milestone log, iterations log,
  discussions log).

Do **not** invoke this skill when the user is asking for help on a
single non-research task (e.g., "fix this bug" or "explain this code").
The methodology is for research projects, not for general engineering.

## How to invoke

1. Read `agent-instructions.md` in this directory for the agent's
   behavioural protocol.
2. Read `invocation-examples.md` for worked examples of what to do for
   different user requests.
3. Then follow the step-by-step in `../HOW-TO-INVOKE.md`.

## Files in this skill directory

| File | Purpose |
|---|---|
| `SKILL.md` | This file. Skill description and trigger phrases. |
| `agent-instructions.md` | Behavioural protocol for an agent loaded with this repo as context. |
| `invocation-examples.md` | Worked examples of common user requests → agent actions. |

## How this skill relates to the rest of the repo

This skill is the **entry point**. The substance lives in:

- `../PHILOSOPHY.md` — the worldview.
- `../playbooks/` — 13 numbered operational playbooks.
- `../templates/` — copy-paste skeletons.
- `../case-studies/` — abstracted examples from the seed session.

The skill does not replace those. It tells the agent how to *use* them.
