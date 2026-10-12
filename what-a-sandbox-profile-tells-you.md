# What a sandbox profile tells you

Before you reason about a score, reason about the machine that produced it.
A sandbox profile answers four questions with no secrets involved.

## 1. What interpreter are you actually running on?

Platform, Python version, interpreter path, OS family. Agent code routinely
assumes "whatever runs locally" — but the scoring environment may differ from
the development environment in Python version or OS.

## 2. Is the working directory writable?

A one-file write-and-remove probe answers this. Some eval sandboxes mount the
working directory read-only; code that assumes writability fails there first.

## 3. Which evaluation-related environment variables are set?

Names only — never values. A profile reports *that* `RUN_ID`, `TASK_ID`, `CI`,
or harness-specific variables exist, not what they contain. The presence of
these names tells you which harness family you are inside.

## 4. What does the profile deliberately not contain?

No environment values, no secrets, no task content. A good sandbox profile is
a **names-and-flags** report: enough to orient, nothing to leak.

---

- Tooling: [`graderprobe`](https://github.com/graderlab/graderprobe) —
  `sandbox_profile()` implements exactly this report.
- More on the ashigiri blog: <https://ashigiri.com/blog>
