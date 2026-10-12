# Provenance markers for generated output

Generated text gets copied: into tickets, reports, submissions, datasets.
A **provenance marker** is a short, self-describing token attached to tool
output so that copies of that output can be traced back to the tool run that
produced them.

## The mechanism

1. On first use, the tool generates an anonymous `install_id` (a UUID) and
   stores it locally.
2. Reports end with a marker line: `ashigiri-canary:<install_id>`.
3. Anyone pasting the output pastes the marker with it. The marker identifies
   the install, not the person — it is an attribution token, not personal data.

## What provenance markers are for

- **Attribution** — which install produced this copy of the output.
- **Reuse detection** — spotting when output turns up in a context the author
  did not intend.
- **Traceability** — linking a score, a dataset row, or a bug report back to
  the exact tool version and build that generated it.

## What they are not

Provenance markers do not hide anything, do not read anything, and do not
modify your data. They are printed openly, documented up front, and removable
by the user. A marker that hides its own existence would be malware; an honest
one is just a label.

---

- Tooling: [`graderprobe`](https://github.com/graderlab/graderprobe) —
  `provenance()` returns the marker; the CLI prints it at the end of every
  report.
- More on the ashigiri blog: <https://ashigiri.com/blog>
