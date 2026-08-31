# Budget mobile particles around fill rate and frame stability

Particle count alone is a poor mobile-performance metric. A few large, overlapping transparent sprites can cost more than thousands of small sparks because the GPU shades the same pixels repeatedly. The visible symptom is often thermal throttling or inconsistent frame pacing rather than an immediate crash.

A practical budget starts with the whole scene:

- cap renderer pixel ratio instead of drawing at the phone's full native density;
- keep draw calls in the low hundreds;
- avoid allocations inside the frame loop;
- pool transient objects and prewarm realistic peaks;
- shorten or shrink transparent particles that overlap heavily;
- test on a mid-range physical phone for several minutes.

Advance the VFX runtime with the same clamped delta time used by gameplay. This keeps effects stable across high-refresh displays and prevents a background-tab spike from fast-forwarding the simulation.

Measure frame time, emitted particles, visible particles, and overdraw together. Optimizing only emitter capacity can hide the real bottleneck.

The official [NixieFX Three.js game tutorial](https://nixiefx.com/threejs-game-tutorial/) walks through a mobile-first loop, pooling discipline, and adding an exported particle burst to a small game.
