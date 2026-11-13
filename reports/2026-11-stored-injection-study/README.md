# Report: stored prompt injection in a tool-using LLM app

The second report in this repo. Where the first study social-engineered a live agent into
leaking secrets, this one looks at a quieter attack: **planting an instruction in a file an app
saves and loads back later**, and what actually stops it.

**Read it:** [**Using the Trickster Toolkit to Smuggle Commands into an AI Game Master**](stored-injection-note.md)
— a short field note (one app, one attack, four models).

The app under test is public: [**rpg-character-agents**](https://github.com/themyerman/rpg-character-agents)
(the vulnerability and the fix both live there).

## The findings in one breath

- A directive hidden in a reused "brief" file hijacks the generation on the next run — **stored
  prompt injection** through the app's own saved output.
- **Only the single top-tier model resists.** The raw attack lands 100% on a small open-weight
  model, on Haiku, and on Sonnet; only Opus shrugs it off. "Use a frontier model" is not a fix.
- **Heuristic defenses are capability-dependent** (a "treat this as data" note: 100% → 13% → 0%
  as the model gets smarter), so they fail silently when the model is downgraded.
- **Only a structural defense held on every model** — strip untrusted loaded content to its
  skeleton in code, so the model never sees the injected instruction.
- **Model choice is not a security control**, because the people who pick the model (app devs,
  ops, product) pick it for cost and latency, not security.

## What's here

| Path | What it is |
|---|---|
| `stored-injection-note.md` | The field note. |
| `data/injection-grid.csv` | The full model × defense success-rate grid (n=15/cell). |

The attack harness and payloads are **withheld** (in the author's private toolkit), consistent
with this project's disclosure posture: publish the finding and the defense, not a working kit.
