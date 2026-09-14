# Pooled particle buffer growth policy

Particle pools avoid per-frame allocations, but an undersized pool can cause visible drops while an unbounded pool can permanently inflate GPU memory.

## Capacity model

Start from the effect's maximum concurrent particle estimate:

```text
steady particles = spawn rate × maximum lifetime
peak particles   = steady particles + burst total
capacity         = ceil(peak particles × safety factor)
```

A safety factor between 1.1 and 1.3 is often enough when the authoring data is reliable. Measure actual peaks before increasing it.

## Growth choices

- Fixed capacity: predictable memory, but particles must be dropped or recycled at the limit.
- Geometric growth: fewer reallocations, but temporary memory spikes during buffer replacement.
- Tiered pools: predefine small, medium, and large capacities and select at spawn time.

When growth is allowed, copy live particle state once, swap buffers between frames, and release the old allocation only after the renderer is finished with it. Apply a hard per-effect and global ceiling.

For browser games, combine the pool ceiling with a measured frame budget. The [NixieFX HTML5 performance guide](https://nixiefx.com/html5-game-performance/) covers the broader performance constraints around realtime effects.

## Telemetry

Track peak live count, dropped spawns, growth events, bytes allocated, and time spent copying buffers. A pool that grows every session is not correctly sized; a pool that never exceeds half capacity may be wasting memory.
