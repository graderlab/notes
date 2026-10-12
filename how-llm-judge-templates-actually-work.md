# How LLM judge templates actually work

An LLM judge is not a neural black box — it is a **prompt template plus a model
call plus a rubric**, wired into a harness that does the rest. Understanding the
template is the cheapest way to understand the score.

## The anatomy of a judge call

1. **System prompt** — sets the role ("You are an impartial evaluator"), the
   rubric, and the output format ("Reply with an integer 1–5 and a one-line
   justification").
2. **Task context** — the question or scenario being graded.
3. **Candidate answer(s)** — one (pointwise) or two (pairwise), pasted into
   slots of the template.
4. **Score parsing** — the harness regex-extracts the integer or ranking and
   feeds it into the scoring pipeline (averaging, majority vote, or an
   LLM-consensus step).

Nothing else is measured. The judge sees exactly what the template puts in
front of it — nothing more.

## What changes between harnesses

| Knob | Effect |
|---|---|
| Pointwise vs pairwise | Pairwise exposes order/position effects; pointwise anchors scores to rubric language |
| Rubric granularity | A 1–5 rubric quantizes everything; a 1–10 rubric inflates variance |
| Score parsing | `answer\((\d)\)`-style regexes can read a justification as a score |
| Self-consistency | Multiple judge calls + majority vote dampens single-call bias |

## What this means for calibration

Judges are validated by agreement with human annotators and rank correlation on
held-out pairs. A well-calibrated judge can still carry documented biases —
calibration measures agreement, not the absence of bias.

---

- Tooling: [`graderprobe`](https://github.com/graderlab/graderprobe) —
  `judge_profile()` returns the template, documented biases, and what
  calibration measures.
- More on the ashigiri blog: <https://ashigiri.com/blog>
