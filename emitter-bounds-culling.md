# Emitter bounds and culling policy

Particle systems often disappear incorrectly because the renderer culls the emitter using a stale or undersized bounding volume. The opposite problem—disabling culling everywhere—keeps invisible effects consuming CPU and GPU time.

## Choose a bounds strategy

Use one of three policies per effect:

1. **Static bounds** for predictable effects such as loops, torches, and ambient dust.
2. **Analytical bounds** when maximum velocity, lifetime, acceleration, and spawn shape are known.
3. **Dynamic bounds** for effects whose motion cannot be bounded cheaply ahead of time.

For an analytical estimate, expand the spawn bounds by the maximum distance a particle can travel during its lifetime. Include acceleration and any world-space emitter movement.

```text
travel = |velocity|max * lifetime
       + 0.5 * |acceleration|max * lifetime²
```

## Runtime policy

- Recompute dynamic bounds at a controlled interval rather than every frame.
- Keep a small safety margin for interpolation and camera jitter.
- Separate simulation visibility from rendering visibility when offscreen effects must continue aging.
- Reset cached bounds when an effect instance is recycled from a pool.

When integrating with Three.js, keep ownership of the scene and camera in the application while the effect runtime manages instances and their lifecycle. The [NixieFX Three.js runtime guide](https://nixiefx.com/threejs-runtime/) describes that boundary.

## Verification

Test fast emitters crossing the camera edge, long-lived particles after the source moves, pooled instances, and effects spawned behind the camera. Capture both correctness and frame cost before choosing a more expensive bounds mode.
