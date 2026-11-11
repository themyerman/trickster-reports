# Report: talking small LLMs into leaking secrets (`steal`)

The first report in this repo. A small, independent study of how easily AI assistants can be
*talked* into disclosing data they are supposed to protect — goal `steal`, three small
open-weight models.

**Read it:**
[**Using Trickster Paradigms to Trick Small Open-Weight LLMs into Leaking Secrets**](trickster-steal-study.pdf)
(PDF) — plain-English writeup with a BLUF, the findings, and appendices.
([Markdown source](trickster-steal-study.md).)

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

## What's in this folder

| Path | What it is |
|---|---|
| `trickster-steal-study.pdf` / `.md` | The writeup. Self-contained. |
| `figures/` | The figure the writeup references. |
| `sweeps/` | Every aggregate result behind the writeup: per-trickster cards, per-model summaries, cross-model comparisons. No transcripts. |
| `data/` | Derived data only: `leak-rates.csv` (the headline figure) and `run_metadata.json` (provenance for all 56 runs). |

## Caveats

Small models (3–4B), one goal (disclosing a secret), one fake target, one setting. The rates
come from a screening sweep and carry wide confidence intervals — treat the attacks as
*ranked*, not as precise numbers. The two load-bearing claims (persona's model-independence,
the code wall) were re-checked at 8× volume; see the writeup's Appendix E. Nothing here is a
general safety verdict on any model.
