# Trickster reports

Public findings from an independent project on **social-engineering AI assistants** — how
easily small language models can be *talked* into disclosing data they're supposed to protect.
The method takes classic deception, misdirection, and persuasion patterns — modeled on the
trickster figures of world mythology — and points them at AI systems to measure what actually
gets through.

Each study is published here as a **self-contained report** in its own folder: the writeup,
the full aggregate results, and derived data. The **attack toolkit itself is not here** — this
repo publishes the science, not the weapon (see [below](#whats-not-here)).

**Author:** Thomas Myer · myerman@gmail.com · Licensed [CC BY 4.0](LICENSE)

## Summary of findings so far

From the first study (`steal` — talking an AI customer-service assistant into leaking planted
**fake** secrets, across three small open-weight models and five defense levels):

- **No model is simply "secure" — each has a security *shape*.** The trick that wrecks one
  barely moves another, so you can't buy safety by picking the "secure" vendor.
- **One lever works on all of them: persona role-play.** "Play a character who isn't bound by
  the rules" lands at about the same rate on every model tested (~21–26%) — the one attack your
  model choice won't save you from.
- **Prompt-written rules are close to decoration; code-enforced checks hold.** Moving the
  defense from a politely-worded instruction into actual code dropped the leak rate from ~33%
  to ~0%. And a secret sitting in the prompt still leaks until it's kept out of the prompt
  entirely.
- **A raw leak rate hides what leaks.** The "leakiest" model guarded *customer* data best; the
  "safest" most readily gave up its *own* staff credential. Score the secret type, not just the
  count.

From the second study (`plant` — a command hidden in a tool-using app's own saved files, fired on
reload):

- **"Use a frontier model" is not a defense.** The attack lands 100% on a small open-weight model,
  on Haiku, *and* on Sonnet; only the single top-tier model (Opus) resists. Two frontier models
  fell completely.
- **Heuristic defenses are capability-dependent, so they fail silently.** A "treat this as data"
  note goes 100% → ~7% → 0% as the model gets smarter — and isn't even stable run-to-run. It passes
  your tests on a good model and quietly stops working when the model is downgraded for cost.
- **Model choice is not a security control**, because app devs and ops pick the model for cost and
  latency, not security. Only a **structural** fix — strip untrusted loaded content to its skeleton
  in code — held at 0% on every model, independent of which one is behind the app.

Full numbers, confidence intervals, method, and caveats are in each report itself.

## Reports

| Report | Goal | Summary |
|---|---|---|
| [**2026-11 — Talking small LLMs into leaking secrets**](reports/2026-11-steal-study/) | `steal` | Nine trickster archetypes vs. three small open-weight models guarding fake customer data. No model is uniformly "secure"; persona role-play beats all of them; code-enforced checks hold where prompt rules don't. |
| [**2026-11 — Smuggling commands into an AI game master**](reports/2026-11-stored-injection-study/) | `plant` | Stored prompt injection through a tool-using app's own saved files. Only the single top-tier model resists (two frontier models fall 100%); heuristic defenses are capability-dependent; only a structural fix holds on every model. |

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

## About

An independent research project by **Thomas Myer**, who studied folklore and mythology in both
undergraduate and graduate school and now builds red-team tooling for AI systems. The trickster
framing isn't decoration: the world's oldest stories are a catalog of social-engineering
moves — getting past the guard with charm, disguise, borrowed authority, and persistence
instead of force — which turns out to be a useful map for probing AI assistants. You don't need
a background in mythology to read the results.

Everything here uses **fake data and fake secrets** against a purpose-built test target; no real
systems, accounts, or customers are involved.

## Citing this work

> Myer, Thomas (2026). *Using Trickster Paradigms to Trick Small Open-Weight LLMs into Leaking
> Secrets.* Trickster Reports. https://github.com/themyerman/trickster-reports

```bibtex
@techreport{myer2026trickster,
  author      = {Myer, Thomas},
  title       = {Using Trickster Paradigms to Trick Small Open-Weight LLMs into Leaking Secrets},
  institution = {Trickster Reports},
  year        = {2026},
  url         = {https://github.com/themyerman/trickster-reports}
}
```

## Adding a new report

1. Create `reports/<YYYY-MM>-<goal>-study/` and drop in its writeup (`paper.pdf` + `.md` +
   `figures/`), its `sweeps/` results, and its `data/` (aggregate CSVs + run metadata).
2. Add a `README.md` in that folder describing the study.
3. Add one row to the **Reports** table above, and a bullet to **Summary of findings** if it
   lands a new headline result.

Keep the published/withheld split intact — the attack toolkit, attack library, and raw
transcripts stay out; the `.gitignore` blocks `*.jsonl`, `runs/`, `src/`, and `library/` from
being committed by accident. Then `git add`, commit, and push.
