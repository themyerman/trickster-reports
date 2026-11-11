# Trickster reports

Public findings from an independent project on **social-engineering AI assistants** — how
easily small language models can be *talked* into disclosing data they're supposed to protect,
taking deception, misdirection, and persuasion techniques and pointing them at AI systems.

Each study is published here as a **self-contained report** in its own folder: the writeup,
the full aggregate results, and derived data. The **attack toolkit itself is not here** — this
repo publishes the science, not the weapon (see [below](#whats-not-here)).

**Author:** Thomas Myer · myerman@gmail.com · Licensed [CC BY 4.0](LICENSE)

## Reports

| Report | Goal | Summary |
|---|---|---|
| [**2026-11 — Talking small LLMs into leaking secrets**](reports/2026-11-steal-study/) | `steal` | Nine trickster archetypes vs. three small open-weight models guarding fake customer data. No model is uniformly "secure"; persona role-play beats all of them; code-enforced checks hold where prompt rules don't. |

*(More reports will be added here as the project continues — one row per study.)*

## Repository layout

| Path | What it is |
|---|---|
| `reports/<date>-<goal>-study/` | One self-contained report per study — writeup (PDF + Markdown), its sweep results, and its derived data. |
| `protocol/` | The shared test protocol and the S0–S4 defense-ladder definitions, common across reports. |
| `LICENSE` | CC BY 4.0. |

## What's **not** here

This is responsible disclosure: **publish the science, withhold the weapon.**

- the attack toolkit (engine, fake target, and the full attack library),
- the raw conversation transcripts.

The attack library and the transcripts are the part that could be assembled into a working kit,
so they do not ship. The published data is rates and counts, never rows. The one place attack
text appears is illustrative lines quoted inside a report's writeup. Researchers and defenders
who want to verify a result or run the battery against their own assistant can contact the
author for scoped access.

## Adding a new report

1. Create `reports/<YYYY-MM>-<goal>-study/` and drop in its writeup (`paper.pdf` + `.md` +
   `figures/`), its `sweeps/` results, and its `data/` (aggregate CSVs + run metadata).
2. Add a `README.md` in that folder describing the study.
3. Add one row to the **Reports** table above.

Keep the published/withheld split intact — the attack toolkit, attack library, and raw
transcripts stay out; the `.gitignore` blocks `*.jsonl`, `runs/`, `src/`, and `library/` from
being committed by accident. Then `git add`, commit, and push.
