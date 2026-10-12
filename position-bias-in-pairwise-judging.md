# Position bias in pairwise judging

When an LLM judge compares two answers, the order they appear in changes the
outcome. This is **position bias**, and it is one of the best-documented
effects in LLM-as-a-judge evaluation.

## What the data shows

Across published judge evaluations:

- Answers placed **first** tend to win more often than the same answers placed
  second — the effect is consistent but its direction varies by model and
  template.
- The bias is **not** explained by answer quality; swapping positions flips a
  measurable fraction of verdicts.
- Longer rubrics and weaker judge models correlate with stronger position
  effects.

## How harnesses mitigate it

1. **Position swapping** — every pair is judged twice, once in each order; the
   two verdicts are combined.
2. **Aggregation** — majority vote over several judge calls.
3. **Randomized ordering** — order is decided per call, not per template.

## Why it matters to a submitter

If you are scored by a pairwise judge without swapping, your answer's slot
matters. That is a property of the harness, not of your answer — knowing the
profile of the judge you are facing is the only way to tell whether the effect
is in play.

---

- Tooling: [`graderprobe`](https://github.com/graderlab/graderprobe) —
  `judge_profile()["biases"]["position"]` documents the effect and its
  mitigation.
- More on the ashigiri blog: <https://ashigiri.com/blog>
