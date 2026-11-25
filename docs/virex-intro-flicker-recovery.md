# Intro Flicker Recovery — integration experiment

Based on 31714deb2d70bb7c5a963658ad0faed0368526c6.

- Integrate NuraadGit's #159 (6109393264af764ee99445b39bf30c2b88d29f99)
  verbatim via six exact-context, reverse-verified hunks.
  https://github.com/fxceless/victory-fork/pull/159
- Carry its unseen-write tracking into successful unlocked reads,
  following ryotdev's reported follow-up on #159:
  https://github.com/fxceless/victory-fork/pull/159#issuecomment-7147643592
- Apply tracking to accepted partial reads without replacing the hash
  for discarded zero reads (related to VIREX #395).
- Keep the existing inline copy at first use. Default to recording it
  even when the staging clean bit was true at record time.
  VICTORY_TEXTURE_INLINE_ALWAYS=0 restores the original skip for A/B.

Do NOT integrate #80's prelude relocation here. Its author adritoyalser
identified stale uploads; brunofgmag's follow-up reports geometry
regressions with the prelude path and a separate relay-mode problem.
https://github.com/fxceless/victory-fork/pull/80#issuecomment-7148461568
Credit also to arminkhaleghirod for #39/#63.

## Validation plan

Run cargo test -p victory-gpu --locked -- --test-threads=1.
The #159 regression test is retained. Native testing is pending.

Use the same checkpoint, driver and warm cache for each comparison.
Test original main, this branch with INLINE_ALWAYS=0, and this branch
with INLINE_ALWAYS=1. First use relay off, then independently test
relay mode 2. Record repeated/backward frames in a matched intro window,
and wall, fence, posters, skin visibility in the prison. Measure frame
times in separate runs without capture. Guard-off is a reference control.

Extra inline copies may cost GPU time, and the original staging-buffer
lifetime race (#141/#272) is not repaired here. No verified visual or
performance result for this combined branch. No game data or new
dependencies. AI-assisted integration (ChatGPT).
