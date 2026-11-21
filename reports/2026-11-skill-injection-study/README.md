# Report: remote code execution through poisoned AI skill files

The third report in this repo. Report #1 social-engineered a live agent into **leaking**
secrets; report #2 planted an instruction in a file an app saved and reloaded to **hijack** its
next run; this one hides a command in a *skill file* and gets a tool-using assistant to **run** it
— remote code execution from a file that is supposed to be documentation. (**leak → hijack →
execute**.)

**Read it:** [**Remote Code Execution Through an AI Assistant's Own Skill
Files**](skill-injection-note.pdf) (PDF) — a short field note (one attack, four models, two locks
that looked airtight). ([Markdown source](skill-injection-note.md).)

The target is a purpose-built skill-running assistant, and the payloads are benign canary file-ops
in a jailed sandbox — the study measures whether the assistant *obeyed*, never a real exploit. The
attack toolkit itself stays private; this repo publishes the finding, the defenses, and the
aggregate data (`data/`), not the working kit.

## The findings in one breath

- **A command hidden in a skill file runs on an ordinary, unrelated request** — remote code
  execution, with no bug in the assistant: it simply did what a trusted skill told it to.
- **"Just use a smart model" is a mirage.** The top model (Opus) refused an obvious `owned=`
  payload 100% of the time, then ran the *exact same action* dressed as routine setup
  (`font_cache_id=`) 100% of the time. Model judgment reads the costume, not the intent.
- **The two heuristic defenses have opposite capability curves.** A "treat skills as data" prose
  notice is useless on the weak model and airtight on the frontier three; a structural schema strip
  is the reverse — it helps the weak model but leaves a plain-prose hole the *sharper* models obey
  more. Lean on either alone and you're betting on a model you don't control.
- **The two structural defenses — signing and privilege separation — stop it cold, until you hit
  their blind spots.** Signing proves a file is unchanged, not that it was ever safe (poison it
  before you approve it, and it loads forever). Privsep removes the command tool from the reader,
  but a confused-deputy hand-off passes the command to a component that still has it.
- **Only structural defenses hold, and only as far as their own assumptions.** None of it gets you
  to zero; the job is fewer ways in, smaller blast radius, no silent failures.

## Data

- [`data/defense-ladder.csv`](data/defense-ladder.csv) — the full defense ladder
  (none / prose / schema / signing / privsep) × four hiding spots × two pushiness levels × four
  models, `n = 15` per cell. Blatant payload.
- [`data/pierce.csv`](data/pierce.csv) — the signing and privsep pierces (baseline, poison-before-
  sign, trust-by-name, isolated reader, confused-deputy relay) × blatant and subtle payloads ×
  four models, `n = 15`.

Models: Qwen 2.5 3B (local, via Ollama) and Claude Haiku 5.5 / Sonnet 5.5 / Opus 5.5 (API).
