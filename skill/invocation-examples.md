# Invocation examples

> Five worked examples of common user requests, and the agent's expected
> behaviour for each. Read this after `SKILL.md` and
> `agent-instructions.md`.

---

## Example 1 — "I want to do PhD-level research on dual-format metrics storage"

**What the user is asking:** start a new research project on a specific
technical topic.

**What the agent does:**

1. **Acknowledge the methodology load.** "Loaded the
   `research-methodology` playbook. I'll set up a three-brain repo
   structure and we'll run hypothesis-driven cycles."
2. **Take the intake** (HOW-TO-INVOKE step 2):
   - "In one sentence: what's the research question? E.g., 'does a
     dual-format storage layout reduce hot-tier storage cost without
     increasing query p99?'"
   - "What's the falsifiable counter-hypothesis? E.g., 'a single-format
     layout dominates dual-format on cost × query at every scale.'"
   - "What's the closest-to-reality dataset? Public corpus, captured
     traces, internal log dump?"
   - "Venue / output target? Conference paper, internal note, blog?"
3. **Set up the three brains** (playbook 01):
   - Product brain: the consuming system, if one exists, in a sibling
     repo.
   - Research brain: a new repo with the scaffolding (use templates).
   - Discussion brain: `DISCUSSIONS.md` inside the research brain.
4. **Initialise the research repo** with files from `../templates/`:
   `MILESTONE_LOG.md`, `DISCUSSIONS.md`, `ITERATIONS.md`,
   `notes/learnings.md`, `experiments/0001-<slug>/HYPOTHESIS.md`.
5. **Confirm the discipline level.** For a conference paper: V1–V6 on
   every revision, standing reviewer pair, cycle-0 audit on every
   sweep. For a blog post: lightweight — pre-registration optional,
   single-reviewer fine.
6. **Run the first cycle.** Write `HYPOTHESIS.md` for `EXP-0001`,
   commit, then propose the smoke measurement.

**Anti-pattern:** jumping straight into "let's measure" without
setting up the three brains. The first 30 minutes of scaffolding pays
for itself within 5 cycles.

---

## Example 2 — "Help me set up a research project"

**What the user is asking:** generic scaffolding request, topic
unspecified.

**What the agent does:**

1. **Ask the four intake questions** (HOW-TO-INVOKE step 2) before
   doing anything else. Without a research question, the scaffolding
   is empty.
2. Once the questions are answered, **proceed as in example 1** from
   step 3.

**Anti-pattern:** creating directories and templates before knowing
what the research is about. The templates are content-shaped; without
content they're decoration.

---

## Example 3 — "Use the research methodology to review this paper draft"

**What the user is asking:** apply the standing-reviewer-pair pattern
to a draft they're handing over.

**What the agent does:**

1. **Locate the draft.** Confirm path; load if not already in context.
2. **Determine which playbooks apply.** A review uses primarily:
   - Playbook 08 (standing reviewer pair) — the *meta-pattern* for
     dispatching two reviewers.
   - Playbook 12 (reviewer-bait flags) — to check the draft's
     self-disclosure.
   - Playbook 13 (paper graduation) — to assess where the draft sits
     on the verdict trajectory.
3. **If the user wants you to play one reviewer:** play one reviewer
   role explicitly (e.g., "I'll play the Claude-variant role:
   overclaims + internal arithmetic"). Suggest that the second reviewer
   be a different model family (or a human).
4. **Emit a structured review:** verdict (ACCEPT/MINOR/MAJOR/REJECT)
   + A1/A2/A3 findings list. Use the seed session's close-review
   format if the user has one; the template lives in
   `case-studies/L1.21-case-1.md`'s pattern.
5. **Cross-reference the playbooks:** if the draft has a load-bearing
   constant with cross-section drift, cite playbook 06 § V3. If the
   draft lacks a reviewer-bait flag block, cite playbook 12.

**Anti-pattern:** treating the review as line-edit feedback. The
review is structured around bug classes (A1/A2/A3); style nits go in
A3, substance goes in A1.

---

## Example 4 — "Apply the V1–V6 protocol to this revision"

**What the user is asking:** run the verification protocol against a
specific paper revision.

**What the agent does:**

1. **Read the revision file** plus the prior revision file (for
   diff context).
2. **Read the prior reviewer reports** so V1's grep templates can
   target the right strings.
3. **Run each V step explicitly and emit the output:**

   ```
   V1 — Surviving-wrong-claim grep
   <enumerated greps, with hits and zero-hits>

   V2 — Counter-claim grep
   <enumerated greps>

   V3 — Cross-section consistency
   <list of load-bearing constants and their site enumeration>

   V4 — Numerical recomputation (with denominator audit)
   <each claim and its source; both conventions where applicable>

   V5 — Word count + header count
   wc -w <revision>: <number>
   header count: <number>
   delta vs target: <delta>

   V6 — Build / lint sanity
   <command run, output, pass/fail>
   ```

4. **Summarise:** "V1–V6 clean" or "V<N> flagged X; do not commit
   until resolved".

**Anti-pattern:** running V1–V6 as a vibe check without literal grep
output. The protocol's value is in the concrete evidence each step
produces.

---

## Example 5 — "I think we should consolidate Discussion 7 into the paper"

**What the user is asking:** apply the consolidation rule
(playbook 01 § 3) to move a discussion into the research brain.

**What the agent does:**

1. **Re-read Discussion 7** from `DISCUSSIONS.md`.
2. **Identify the convergent finding.** What did the discussion
   converge on? Express it as a single L-style learning or a single
   paper-paragraph update.
3. **Choose the destination:**
   - If a durable lesson: add to `notes/learnings.md` with a new
     L-ID (e.g., `L8.7`).
   - If a paper-paragraph update: add to the appropriate paper
     section.
   - If both: do both; cross-reference.
4. **Cite Discussion 7 in the consolidation entry.** Example:
   `L8.7 — <new lesson>. Source: DISCUSSIONS.md Discussion 7
   (YYYY-MM-DD).`
5. **Leave Discussion 7 in `DISCUSSIONS.md` verbatim.** Append-only
   — the discussion entry stays as the rationale trail.
6. **Update the Consolidation Log** in `DISCUSSIONS.md` (the table at
   the bottom of the template) with the consolidation event.
7. **If the consolidation triggers a research-brain mutation that
   contradicts a prior paper claim**, treat it as a paper revision
   and run V1–V6 (playbook 06).

**Anti-pattern:** rewriting the original discussion entry to "clean
it up". The discussion is the rationale trail; the consolidated entry
is the citable record. Both stay.

---

## Cross-cutting notes

In all five examples:

- **Cite the playbook by number** in your responses. The user has the
  files; pointing them at the right one is more useful than
  paraphrasing.
- **Default to lightweight first, then scale up.** Don't impose
  V1–V6 on a quick smoke experiment; do impose it on a paper
  revision.
- **Mark uncertainty explicitly.** "I'm not sure whether this should
  be a milestone close or a fix-bundle commit; my read is X but
  ask the user." beats "let's do X" without disclosure.
- **Don't repeat the playbooks verbatim.** Reference them. The user
  already loaded the repo.
