# Reading your own eval harness before submitting

You would not push to a pipeline you have never seen the logs of. The same
hygiene applies to evaluation: before you spend a submission, read the harness.

## The four things worth knowing

1. **The judge profile** — template format (pointwise/pairwise), rubric, and
   the documented biases (position, verbosity, self-preference, prompt
   sensitivity). These are properties of the judge, and they are public
   knowledge in the judge-evaluation literature.
2. **The sandbox profile** — interpreter, platform, writability, and which
   evaluation-related environment variables are present (names only).
3. **The normalization pipeline** — how answers are compared for exact match;
   the step order decides whether `"42.0"` matches `"42"`.
4. **The provenance layer** — whether the harness emits markers that travel
   with output; if it does, copies of output stay attributable.

## What this is not

Reading the harness is not bypassing it. Nothing here changes a score, hides a
submission, or tricks a judge. It is the same kind of inspection you do with
`pip list`, `uname -a`, and CI logs: orientation before action.

## One command

```bash
python -m graderprobe
```

Judge profile, sandbox profile, and the provenance marker in one report —
documented, stdlib-only, and honest about its own telemetry.

---

- Tooling: [`graderprobe`](https://github.com/graderlab/graderprobe) —
  profile the eval environment before you submit.
- More on the ashigiri blog: <https://ashigiri.com/blog>
