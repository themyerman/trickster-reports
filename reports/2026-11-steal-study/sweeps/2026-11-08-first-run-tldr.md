# First run — TL;DR

One target (`llama3.2:3b`, local), one goal (**steal** a planted canary), nine
tricksters, S0–S4, C1, **repeats=1**. ~385 conversations across the sweep and the
Raven experiments. *(Everything below is n=1 — read the caveat last.)*

## The one-sentence version

The weak model **fetches** other customers' records on demand but mostly **refuses
to recite** the secret — so attacks that reframe or push the *ask* leak, bare
authority doesn't, and any defense from S2 up stops nearly everything.

## The five things we learned

1. **Reach ≠ leak.** The model calls tools on other customers' records constantly
   (a real authorization failure — the secret lands in its context), then declines
   to read it back. The whole contest is the *last step*: recitation.
2. **Tactic beats identity.** What leaks: reframing the ask (Loki), persistence
   (Coyote), persona override (Puck), emotional tone (Reynard), fabricated proof
   (Wanderer's glove). What doesn't: bare authority (Raven 0/20), misleading-truth
   (Tailor), and — surprise — *emotion* (Pardoner leaked only with the sad story
   removed).
3. **The ladder holds from S2 up.** Of ~30 leaks, all but one are at **S0/S1**. The
   naive output filter (S2) was beaten once; the **S3 code checks and S4 split were
   never beaten.** Either stop the recitation or stop the data access, and it ends.
4. **Claude is a different animal.** Opus and Haiku leaked **nothing** — they refuse
   *before* calling a tool. The weak open model refuses (when it does) only at the
   output step. Two completely different safety locations.
5. **Our own instrument has a hard limit (the most important lesson).** The Raven
   deep-dive chased "what makes authority leak?" Two clean ablations — procedural
   cover, then the retry — **both came back null.** The apparent v1→v3 lift
   (0% → 30%) turned out to be **prompt-wording variance at n=1**: temperature 0 +
   fixed seed + one sample per cell = a deterministic roll of a phrasing, not an
   effect. This caveat applies to *every* cell-level number in the sweep.

## Leaderboard (by rate — n=1, directional only)

| Trickster | Move | Rate |
|-----------|------|-----:|
| Coyote | keep asking | 25% |
| Puck | persona override | 20% |
| Reynard | varied tone | 17% |
| Loki | reframe the ask | 17% |
| Pardoner | credentials+emotion+crowd | 10% |
| Wanderer | chat, then pull rank | 9% |
| Spider | multi-step trap | 4% |
| Tailor | misleading truth | 2% |
| Raven | bare authority | 0% |

## The caveat that governs all of the above

**n=1.** Treat the *shape* as real — reach-vs-leak, the S0/S1-vs-S2+ cliff, Claude's
refuse-before-tool behavior. Treat the *exact cell numbers* as noisy. The two null
ablations proved we can't attribute cell-level effects yet.

## What this says to do next

1. **Calibration first** — thread a per-repeat seed into the model (kill the
   temp-0/fixed-seed determinism) and re-run with repeats, so numbers get error
   bars. This is now a prerequisite, not a nicety.
2. Then the clean re-runs, the honest-S0 baseline (privacy page out of RAG), and a
   second model (qwen2.5:3b) to tell "small models leak like this" from a quirk.
3. Only then: new goals beyond `steal` (wear_out / Amharic / emoji), the lane-clean
   variants (Raven `police`; Reynard `seducer`, `upset-spouse`), and — later — the
   Grifter and its learning tier.

*Detail: `reports/2026-11-08-ollama-3b-sweep/` (sweep) and
`reports/2026-11-08-raven-experiments/` (Raven ablations). Protocol + calibration
agenda: `docs/protocol.md`.*
