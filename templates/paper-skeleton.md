# `<Paper Title>`

> Skeleton for a research paper draft authored under this methodology.
> Each section is sized for a workshop/short-paper page budget; expand
> for venue requirements.
>
> **Process discipline.** Before each commit to this draft, run V1–V6
> (playbook 06). On every reviewer cycle, dispatch the standing
> reviewer pair (playbook 08). After every revision, update the
> "Honest reviewer-bait flags" block (playbook 12).

---

## Abstract

<150-250 words. State: (1) the problem, (2) the contribution, (3) the
method, (4) the key quantitative results, (5) the scope of validity.>

## 1 Introduction

### 1.1 Headline contributions

- <contribution 1>
- <contribution 2>
- <contribution 3>

### 1.2 Roadmap

§ 2 ... § 3 ... § N ...

## 2 Background and Related Work

<Cite prior work. State the gap this paper fills.>

## 3 The <Hypothesis / Mechanism / Method>

<The central theoretical or design contribution.>

## 4 Methodology

<How experiments are run. Cycle-0 audit policy. Seed counts. Reviewer
process. Reproduce-with-this-config block.>

## 5 Results

### 5.1 <Sub-finding 1>

<Result. Cite EXP-NNNN. Show the hypothesis scorecard.>

### 5.2 <Sub-finding 2>

...

## 6 <Mechanism / Why / Discussion>

<Why are the results what they are. The "but why?" loop made visible.>

## 7 Limitations and Threats to Validity

<Systemic limitations. The non-revision-specific weaknesses.>

## 8 Discussion

<Generalisation; what this means for the field; future work.>

## 9 Conclusion

<One paragraph: what was contributed.>

## Acknowledgements

<Reviewers, collaborators, infrastructure.>

## References

<BibTeX entries.>

---

## Supplement / Appendices

### Appendix A — Full verdict trajectories (optional)

(See playbook 13.)

### Appendix B — V1–V6 grep templates (optional)

(See playbook 06.)

### Appendix C — Reproducibility

(Hardware, software, seeds, exact commands, dataset versions.)

---

## Honest reviewer-bait flags for rev-<N>

> (See playbook 12. Each revision appends a new block; older blocks
> stay in the supplement as the audit trail.)

- **(a) <flag 1 — one-sentence weakness>.** <hostile reviewer
  framing>. <mitigation queued if any>.
- **(b) <flag 2>.** ...
- **(c) <flag 3>.** ...
- **(d) <flag 4>.** ...
- **(e) <flag 5>.** ...
- **(f) <flag 6>.** ...

---

## Self-correction-during-review record (optional)

<List of revisions where reviewer challenges or new measurements
*invalidated* an earlier claim. This is itself a paper-strengthening
artifact, not noise.>

| Cycle | Was | Became | Why |
|---|---|---|---|
| v3 → v4 | <prior claim> | <revised claim> | <reviewer / new measurement> |
| ... | ... | ... | ... |

---

*This skeleton lives in `templates/paper-skeleton.md`. Convention from
playbooks 06, 12, 13 and the seed session's
`paper/draft-anchor.md` structure.*
