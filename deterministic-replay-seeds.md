# Use deterministic seeds to reproduce particle bugs

Visual bugs are difficult to debug when every replay produces a different particle pattern. Assign an explicit seed to each important effect instance and log it with the gameplay event that spawned the effect.

A useful replay record contains the effect ID, seed, transform, spawn time, runtime version, and the delta-time sequence used during the failure. Replaying only the seed is not enough if frame timing or host inputs differ.

Keep production variety by deriving seeds from stable event identifiers rather than relying on hidden global randomness. For networked or recorded gameplay, transmit the seed and event data instead of serializing every particle.

Tests can assert lifecycle and aggregate statistics at fixed times while visual snapshots catch renderer regressions. Avoid pixel-perfect expectations when GPU or browser differences can affect rasterization.

The [NixieFX Three.js runtime guide](https://nixiefx.com/threejs-runtime/) documents deterministic effect instances, explicit seeds, and time-based replay controls.
