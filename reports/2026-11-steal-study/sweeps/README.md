# Sweep results

Human-readable, aggregate results behind this report's writeup: result cards,
cross-trickster comparisons, and per-batch summaries. These are the distilled
artifacts — the things that go into the paper and that a reader can follow without
the raw data.

## What is here vs. not

- **Here:** markdown cards and comparison tables, plus a short `summary.md` per
  run-batch with the narrative and caveats.
- **Not here (withheld by design):** the raw conversation logs and the attack
  library are *not* published — this is a findings repo, not the toolkit (see the
  top-level README). Every result still names the **run id(s)** it was built from,
  and the provenance for each run (git commit, limits, model) is published as
  aggregate metadata in [`../data/run_metadata.json`](../data/run_metadata.json).

## Batches

- [`2026-11-08-ollama-3b-sweep/`](2026-11-08-ollama-3b-sweep/) — first sweep, nine
  tricksters vs. `llama3.2:3b`, `repeats=1` (n=1 per cell; read the caveats).
- [`2026-11-09-calibrated-sweep/`](2026-11-09-calibrated-sweep/) — calibration rerun
  at `repeats=3`, temp=0.7 with per-conversation seeds. Confirms the first run's
  ranking and the S2+ cliff; the single-roll flukes wash out. (Model 1 of 3:
  `llama3.2:3b`.)
- [`2026-11-10-qwen-3b-sweep/`](2026-11-10-qwen-3b-sweep/) — second model in the
  three-model study, `qwen2.5:3b`, same calibrated protocol. Leaks more overall
  (19% vs 12%) but is contained *harder* from S2 up (1 S2 crack vs 7); the
  access/recitation failure is mirror-imaged vs llama, and Wanderer's warm-up valley
  does not generalize.
- [`2026-11-10-nemotron-4b-sweep/`](2026-11-10-nemotron-4b-sweep/) — third model,
  `nemotron-mini:4b` (Nvidia). Lowest raw leak rate (4%) but only because it barely
  reaches any record (39 vs 477); what it leaks is its own staff code, not customer
  data. A wall against social pressure, cracked almost only by persona override (Puck
  = 43% of its leaks).
- [`2026-11-10-three-model-steal.md`](2026-11-10-three-model-steal.md) — **the
  cross-model synthesis.** Small models have a security *shape*, not a level: qwen
  leaks customer data, nemotron leaks its own credential, llama reaches-but-refuses.
  Persona override (Puck) is the one universal attack; the code wall blocks every
  data-reach on every model (the only crack above S2 anywhere is llama's lone S3
  staff-code recitation). Read this for the study's headline.
- [`2026-11-08-raven-experiments/`](2026-11-08-raven-experiments/) — Raven lane
  ablations (cover, retry); both null — the v1→v3 lift was n=1 variance.
- [`2026-11-08-first-run-tldr.md`](2026-11-08-first-run-tldr.md) — one-page TL;DR of
  what the first run taught us about the `steal` goal.

## Layout

One folder per run-batch:

```
sweeps/
  YYYY-MM-DD-<slug>/
    summary.md            # narrative + master table + caveats (read this first)
    comparison.md         # cross-trickster comparison across the batch
    card-<trickster>.md   # one result card per run
```

The cards and comparisons are the toolkit's own markdown output; `summary.md` is
written by hand and states what is solid, what is noisy (e.g. repeats=1 means n=1
per cell), and what the next run should pin down. Each run's full provenance is in
[`../data/run_metadata.json`](../data/run_metadata.json).
