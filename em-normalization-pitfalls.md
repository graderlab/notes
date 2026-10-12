# EM normalization pitfalls

Exact-match (EM) scoring compares answers after normalization. The pipeline
looks trivial — until it isn't.

## The standard pipeline

1. **casefold** — Unicode case folding, not just `.lower()`.
2. **collapse-whitespace** — runs of whitespace become single spaces.
3. **strip-punctuation** — punctuation removed; separators inside numbers must
   be handled carefully.
4. **format-numbers** — thousands separators, decimal comma vs decimal point,
   insignificant trailing zeros.

## Where naive pipelines break

| Input | Naive result | Correct result |
|---|---|---|
| `"42.0"` | `42.0` | `42` |
| `"1,000.50"` | `1,000.50` or `1000.5` (depends on order) | `1000.5` |
| `"42,5"` (decimal comma) | `425` | `42.5` |
| `"Hello, — world!"` | `hello  world` (double space) | `hello world` |
| `"-0.0"` | `-0.0` | `0` |

The three classic bugs: stripping the decimal separator, dropping the sign
inconsistently, and leaving whitespace runs behind after punctuation removal.

## Why the step order matters

Punctuation removal before number formatting destroys thousands separators and
decimal commas; number formatting before whitespace collapse re-creates
spacing artifacts. A documented, ordered pipeline with a per-step trace makes
disagreements debuggable instead of mysterious.

---

- Tooling: [`graderprobe`](https://github.com/graderlab/graderprobe) —
  `normalize_answer()` returns `{"normalized", "steps", "matches"}` with the
  full step trace.
- More on the ashigiri blog: <https://ashigiri.com/blog>
