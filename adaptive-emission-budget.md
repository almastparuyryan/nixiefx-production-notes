# Adaptive Emission Budget

Particle emission can adapt to device load without changing the visual identity of an effect. The key is to reduce newly spawned work gradually while preserving important bursts and existing particle motion.

## Controller

Measure a smoothed frame-time signal over a short window. Compare it with two thresholds:

- above the high threshold, decrease the emission multiplier;
- below the low threshold, recover it slowly;
- between thresholds, keep the current value.

Using two thresholds prevents rapid oscillation. Clamp the multiplier to a tested range, and apply a rate limit to both degradation and recovery.

## Preserve visual intent

Prioritize authored events:

1. keep critical impact bursts;
2. preserve particle lifetime and movement where possible;
3. reduce ambient and continuous emission first;
4. simplify secondary trails or sub-emitters next;
5. lower texture or renderer quality only through a separate policy.

Do not retroactively delete random live particles every frame; that creates visible popping. Adapt future emission instead.

## Validation

Test slow ramps, sudden spikes, scene transitions, background-tab recovery, and low-refresh displays. Log the multiplier beside frame time so QA can distinguish adaptive behavior from missing content.

When integrating with Three.js, connect the controller to the effect lifecycle documented in the [NixieFX Three.js runtime guide](https://nixiefx.com/threejs-runtime/).
