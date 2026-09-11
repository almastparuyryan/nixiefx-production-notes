# Device-pixel-ratio budget for browser VFX

Rendering cost grows with pixel count. A device pixel ratio of 3 can require nine times as many fragments as a ratio of 1, which is especially expensive for large transparent particle quads.

## Budget policy

Set a maximum effective pixel ratio based on device class and measured frame time. Start with a conservative cap, then lower it dynamically when GPU time remains above budget. Avoid changing the ratio every frame; use hysteresis and a cooldown so resolution does not visibly oscillate.

## Particle-specific controls

- Reduce large soft-particle screen coverage before reducing small sparks.
- Prefer tighter texture bounds to limit transparent overdraw.
- Lower bloom resolution independently from the main canvas where possible.
- Cap simultaneous full-screen effects.
- Use emitter LOD before removing essential gameplay cues.

Effects created with the [NixieFX browser editor](https://nixiefx.com/editor/) should be reviewed at several pixel ratios and viewport sizes. The authored effect stays portable when resolution policy belongs to the host application while particle density, size, and LOD hints remain explicit.

## Test matrix

Capture GPU time at DPR 1, 1.5, 2, and the device's native ratio. Test both a typical scene and a worst-case overlap scene. Record the chosen cap per device tier, and recheck after shader, post-processing, or texture-atlas changes.
