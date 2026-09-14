# Emitter seed lifecycle

Deterministic particle effects require more than a seeded random-number generator. The seed must have a defined lifecycle across spawning, pooling, looping, and replay.

## Seed ownership

Give each effect instance a base seed supplied by the host or derived from a stable event identifier. Derive independent streams for emitters and sub-emitters so adding one random sample in one subsystem does not perturb every other particle.

```text
instance seed
  -> emitter stream
  -> shape stream
  -> color stream
  -> sub-emitter stream
```

## Lifecycle rules

- Reset streams when a deterministic replay restarts.
- Do not retain the previous seed when a pooled instance is reused unless explicitly requested.
- Define whether each loop repeats the same sequence or derives a new loop seed.
- Serialize the base seed with replay evidence, not necessarily with reusable effect assets.

For Three.js integrations, the host can own the event seed while the effect runtime owns deterministic sampling. The [NixieFX Three.js runtime guide](https://nixiefx.com/threejs-runtime/) describes this application/runtime boundary.

## Tests

Run identical seeds at different render frame rates, recycle instances through a pool, add an unrelated random property, and compare emitted particle state. Determinism should survive those changes unless the effect definition itself changes.
