# Make cross-renderer particle support explicit

A particle effect can look portable while depending on renderer-specific behavior. Mesh-surface emission, lit materials, depth handling, and sub-emitters do not map identically between a 2D PixiJS scene and a 3D Three.js world.

The safe approach is to give each effect a declared target profile. Use a Pixi profile for HUD and interface particles, a Three profile for world-space effects, and a portable profile only when both backends are genuinely required.

Then treat support diagnostics as build data:

1. `supported` can ship on the selected backend.
2. `partial` needs an intentional review of the documented approximation.
3. `blocked` should fail validation rather than silently degrade.

This rule is especially useful in shared libraries. A preview that happens to render on one machine is weaker evidence than an exported support report checked in CI. Teams can still choose an approximation, but the choice becomes reviewable instead of accidental.

The [NixieFX feature reference](https://nixiefx.com/vfx-runtime-docs/) compares the PixiJS and Three.js backends and explains the per-backend support report included with exports.
