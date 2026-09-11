# Spawn-rate normalization across variable frame times

Particle emitters should describe spawn rate in particles per second, not particles per frame. Frame-based emission changes density with refresh rate and produces bursts when the main thread stalls.

## Accumulator pattern

Keep a fractional spawn accumulator per emitter:

```js
accumulator += particlesPerSecond * deltaSeconds;
const spawnCount = Math.floor(accumulator);
accumulator -= spawnCount;
spawn(spawnCount);
```

Clamp `deltaSeconds` before applying the rate so a background-tab pause does not create thousands of particles on resume. A typical clamp is between 50 and 100 milliseconds, but the right value depends on the visual effect.

## Production checks

- Compare 30, 60, 90, and 120 Hz captures over the same wall-clock interval.
- Verify the total spawn count differs only by expected fractional rounding.
- Reset the accumulator when an emitter is restarted intentionally.
- Preserve the accumulator across ordinary render frames.
- Cap the per-frame spawn count to protect recovery from long stalls.

The browser editor and runtime documentation at [NixieFX](https://nixiefx.com/threejs-runtime/) are useful reference points when defining a portable effect contract. Keep authoring controls expressed in time-based units so the same effect behaves predictably in Three.js, PixiJS, and test harnesses.

## Failure signature

If high-refresh devices show denser effects, search for spawn calls tied directly to render-frame count. If resumed tabs burst, inspect the delta clamp and maximum spawn cap before changing the authored rate.
