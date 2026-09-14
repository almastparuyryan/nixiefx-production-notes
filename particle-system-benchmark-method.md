# Particle System Benchmark Method

A useful particle benchmark answers a production question: how many effects can this device sustain under a representative game workload?

## Test matrix

Vary one dimension at a time:

- active particle count;
- spawn rate and burst size;
- transparent overdraw;
- texture resolution;
- material and blending mode;
- simulation work per particle;
- renderer resolution and device pixel ratio.

Run each case after a short warmup. Record median frame time, slow-frame percentiles, memory, and visually observed failures. Frames per second alone can hide instability.

## Scene design

Include both an isolated microbenchmark and a representative scene. The microbenchmark exposes the cost of the effect; the representative scene reveals contention with animation, UI, physics, and post-processing. Keep the camera path deterministic so overdraw is comparable.

## Reporting

State device, browser, viewport, DPR, thermal condition, effect version, and build mode. Separate CPU simulation time from GPU rendering when instrumentation allows. Define a budget before testing—for example, a maximum VFX frame-time contribution—then report the highest configuration that stays inside it.

The [NixieFX HTML5 game performance guide](https://nixiefx.com/html5-game-performance/) provides broader context for fitting particle effects into a browser game's total performance budget.
