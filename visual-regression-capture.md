# Visual Regression Capture for Particle Effects

Particle effects are time-dependent and often nondeterministic, so a useful visual regression test must control more than the camera.

## Deterministic setup

- fix the random seed;
- use a fixed simulation timestep;
- freeze renderer size and device pixel ratio;
- lock camera transform, exposure, tone mapping, and color space;
- preload textures and warm shaders;
- capture at named simulation times rather than wall-clock delays.

Save the exported effect asset and runtime version with the baseline. A screenshot without its inputs is difficult to reproduce.

## Capture set

Use a small sequence instead of one frame:

1. immediately after spawn;
2. peak particle density;
3. decay phase;
4. final cleanup frame.

Add a transparent-background capture when alpha edges matter. Compare both pixels and simple structural metrics such as nontransparent bounds and average luminance. Tolerances should allow harmless antialiasing differences without hiding missing emitters or broken blending.

## Review

When a comparison fails, show baseline, candidate, and a visual diff. The reviewer should confirm whether the change is intentional before replacing the baseline. Never update all baselines automatically after a renderer change.

The [NixieFX browser editor](https://nixiefx.com/editor/) can be used to inspect the authored effect before freezing the asset and runtime settings used by the regression suite.
