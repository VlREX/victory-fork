# World Geometry Recovery — integration test branch

This branch combines three existing public GPL contributions for
fxceless/victory-fork issues #168 and #89. It is NOT a new standalone
fix or a claim that walls are restored.

Original contributions:
- gdawid9407, #191, 1582757f123f5e49f46ca33fc96c782182c3599f:
  https://github.com/fxceless/victory-fork/pull/191
- AscilCH, #206, d3bedc8931b72726136049b50f5abcc02a83d72d:
  https://github.com/fxceless/victory-fork/pull/206
- AscilCH, #207, c75cb22ce82d2d0ec9807b763eedd932c2c2784d:
  https://github.com/fxceless/victory-fork/pull/207

Base 31714deb2d70bb7c5a963658ad0faed0368526c6.
33 nonconflicting hunks applied with exact unique context and
reverse verification. The overlapping host hazard scanner and test
imports were reconciled manually: preserve alias/readback/immediate
writes, add HostRead, prioritize full-settle Run even with no declared
shader write when flights are pending. Saturate read-range endpoints.
Integration commit credits original authors; do not submit duplicate
upstream PRs. AI-assisted conflict resolution.

Validation required on Windows:
    cargo test -p victory-gpu --locked -- --test-threads=1
    cargo clippy -p victory-gpu --all-targets --locked -- -D warnings

A/B: same GPU, driver, warm shader cache, copied checkpoint and camera.
Compare original main versus this branch with identical settings:
defaults, RELAY=2, and LATE_WAIT=3 with the same other switches.
Record wall/window/ceiling/skin presence, dropped draws, mesh groups,
frame time, waits and device loss. Correctness before performance.
No assets or game captures in this repository. Native Rust and
visual testing not performed for this combined branch.
