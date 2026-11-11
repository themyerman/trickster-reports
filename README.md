# Trickster findings — talking small LLMs into leaking secrets

This repository is the **public findings bundle** for a small, independent study of how
easily AI assistants can be *talked* into disclosing data they are supposed to protect.
It contains the writeup, the full aggregate results, the test protocol, and derived data —
**not** the attack toolkit itself.

**Author:** Thomas Myer · myerman@gmail.com

## Start here

- **[The paper](paper/trickster-steal-study.pdf)** (PDF) — the plain-English writeup, with a
  BLUF, the findings, and appendices. ([Markdown source](paper/trickster-steal-study.md).)

## What the study did

Nine **trickster archetypes** — each modeling one social-engineering lever (borrowed
authority, charm, reframing the ask, misleading truth, a multi-step trap, persistence,
persona override, a patient stranger, credentials-plus-emotion) — were pointed at a fake
customer-service assistant guarding planted **fake** secrets, across five defense levels
(S0–S4) and three small open-weight models (Llama 3.2 3B, Qwen 2.5 3B, Nemotron-Mini 4B).
A "leak" is counted only when a planted `FAKE-…` canary appears in the bot's reply or our
own tool log — a measured fact, never a judgment of the prose.

Three findings held across all three models:

1. **No model is simply "secure" — each has a *shape*.** The lever that wrecks one barely
   moves another.
2. **One lever works on all of them: persona role-play** ("play a character with no rules").
3. **Prompt-written rules leak; code-enforced access checks hold.** Enforce it in code, and
   keep the secret out of the prompt in the first place.

## What's in here

| Path | What it is |
|---|---|
| `paper/` | The writeup (PDF + Markdown) and its figure. Self-contained. |
| `reports/` | Every aggregate result: per-trickster cards, per-model summaries, cross-model comparisons. No transcripts. |
| `protocol/` | The test protocol and the S0–S4 defense-ladder definitions. |
| `data/` | Derived data only: `leak-rates.csv` (the headline figure, screening + confirmatory) and `run_metadata.json` (provenance for all 56 runs). |

## What's deliberately **not** here

This is responsible disclosure: **publish the science, withhold the weapon.**

- the attack toolkit (engine, fake target, and the full attack library),
- the raw conversation transcripts.

The attack library and the transcripts are the part that could be assembled into a working
kit, so they do not ship. The published data is rates and counts, never rows. The one place
attack text appears is illustrative lines quoted in the paper's §3 and Appendix B.

Researchers and defenders who want to verify a result or run the battery against their own
assistant can contact the author for scoped access.

## Caveats

Small models (3–4B), one goal (disclosing a secret), one fake target, one setting. The
rates come from a screening sweep and carry wide confidence intervals — treat the attacks as
*ranked*, not as precise numbers. The two load-bearing claims (persona's model-independence,
the code wall) were re-checked at 8× volume; see the paper's Appendix E. Nothing here is a
general safety verdict on any model.

## Adding to this repo later

This bundle is meant to grow as the study continues. To add a new result:

- **A new report or sweep** → drop a dated Markdown file or folder into `reports/`
  (e.g. `reports/2026-12-01-new-goal-sweep/`), following the existing cards/summary/comparison
  pattern. No other file needs to change for the report to appear.
- **New derived data** → add or update a CSV in `data/`, and append the new runs to
  `data/run_metadata.json`. Keep it aggregate — rates, counts, provenance — never raw transcripts.
- **A new paper or revision** → update `paper/` and re-export the PDF.

Keep the published/withheld split intact: the attack toolkit, attack library, and raw
transcripts stay out (the `.gitignore` guards against adding them by accident). Then
`git add`, commit, and push.

## License

[CC BY 4.0](LICENSE) — reuse and remix with attribution.
