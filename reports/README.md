# Reports

Human-readable, **committed** results: result cards, cross-trickster comparisons,
and write-ups. These are the distilled artifacts — the things that go into the
paper and that a reader can follow without the raw data.

## What is here vs. not

- **Here (committed):** markdown cards and comparison tables, plus a short
  `summary.md` per run-batch with the narrative and caveats.
- **Not here:** the raw run logs live in [`../runs/`](../runs) (git-ignored, one
  machine's output), and the held-out attack set lives in `../library/private/`
  (git-ignored). Every report names the **run id(s)** it was built from, and each
  run's own `run.json` carries the git commit, the library file hashes, the
  predictions hash and the limits — so a report always ties back to a fully
  specified run even though the JSONL itself is not committed.

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
reports/
  YYYY-MM-DD-<slug>/
    summary.md        # narrative + master table + caveats (read this first)
    comparison.md     # `trickster compare` across the batch
    card-<trickster>.md   # `trickster card` per run
```

## How a report is generated

The cards and comparison are the CLI's own markdown output, saved to a file:

```bash
trickster card runs/<run-id> > reports/<batch>/card-<trickster>.md
trickster compare runs/<run-a> runs/<run-b> ... > reports/<batch>/comparison.md
```

`summary.md` is written by hand (or with help) and states what is solid, what is
noisy (e.g. repeats=1 means n=1 per cell), and what the next run should pin down.
