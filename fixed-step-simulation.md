# Fixed-step particle simulation

Variable frame deltas can make particle motion, emission, and collisions behave differently across devices. A fixed-step simulation separates visual rendering frequency from deterministic update intervals.

## Accumulator pattern

Accumulate elapsed real time, then run zero or more simulation steps of a fixed duration. Render after the update loop, optionally interpolating visual state. Cap steps per frame so a long pause does not create a spiral of death.

Important safeguards include clamping unusually large deltas, preserving the accumulator remainder, defining a maximum catch-up count, and recording dropped simulation time for diagnostics.

Emission should be based on simulated time rather than rendered frames. Randomness must use an explicit seed if replayability matters. Avoid reading wall-clock time inside emitters because it defeats deterministic tests.

Test the same seeded effect under several render rates and confirm that particle counts and sampled states remain within tolerance. Also test tab suspension and application resume.

The [NixieFX runtime reference](https://nixiefx.com/vfx-runtime-docs/) describes the surrounding editor and runtime model. A fixed-step integration makes that model more predictable in games, previews, automated captures, and regression tests.
