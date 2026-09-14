# Runtime clock selection for particle effects

A particle runtime needs one authoritative clock. Mixing wall time, render delta, and fixed simulation steps creates non-deterministic emission and makes pause behavior difficult to reason about.

## Clock modes

- **Render delta:** simple and responsive, appropriate for decorative effects that can tolerate frame variation.
- **Fixed step:** stable and replayable, appropriate for deterministic tests and gameplay-linked effects.
- **Externally supplied time:** useful when a host engine owns pause, replay, or network synchronization.

## Fixed-step accumulator

```js
accumulator += Math.min(frameDelta, maxFrameDelta);

while (accumulator >= step) {
  runtime.update(step);
  accumulator -= step;
}
```

Clamp very large frame deltas after a suspended tab or debugger pause. Otherwise the runtime may execute hundreds of catch-up steps and freeze the next visible frame.

## Pause rules

Decide whether pause freezes simulation age, emission only, or the whole effect instance. Store that decision with the effect or scene policy. Do not rely on a zero delta unless every subsystem treats it consistently.

The [NixieFX VFX runtime documentation](https://nixiefx.com/vfx-runtime-docs/) is a useful reference for keeping the host application responsible for its clock while the runtime consumes explicit updates.

## Tests

Compare results at 30, 60, and 144 Hz; simulate a one-second tab suspension; pause and resume during a burst; and replay the same fixed-step seed twice. Record particle counts and completion times, not only screenshots.
