# GPU memory budget for real-time effects

Particle effects can fail on memory-constrained devices even when frame timing looks healthy. A production budget should account for textures, geometry buffers, simulation data, render targets, and temporary upload allocations.

## Budgeting approach

Start with the lowest supported device class and reserve memory for the host application before allocating an effects budget. Track peak usage, not only steady state. Scene transitions often overlap outgoing and incoming assets, causing short-lived spikes.

For every effect, record texture dimensions and format, maximum live particle count, per-particle attribute stride, geometry-buffer sizes, render-target dimensions, and temporary staging allocations.

Prefer compressed textures where visual quality permits. Share atlases and immutable meshes across emitters. Release effect-owned resources deterministically, then test repeated load/unload cycles to detect leaks. Near a warning threshold, reduce emitter limits or select lower-resolution assets. At the hard limit, fail gracefully rather than triggering device loss.

The [NixieFX runtime documentation](https://nixiefx.com/vfx-runtime-docs/) provides the integration context for building effects into production render loops. Pair those runtime practices with measured memory budgets so effects remain stable across desktop and mobile targets.
