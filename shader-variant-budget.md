# Shader variant budget for particle materials

Every material feature flag can multiply the number of shader programs a particle renderer must compile. Uncontrolled combinations lead to startup stalls, cache misses, and hard-to-reproduce device-specific failures.

## Define the variant key

Build a stable key from features that genuinely change shader source, such as:

- textured versus untextured particles
- soft particles
- lighting mode
- local or world simulation space
- alpha test
- color-space conversion

Keep numeric values and ordinary uniforms out of the key. They should not create new programs.

## Set a budget

Inventory exported effects in CI and count unique variant keys. Fail or warn when a project exceeds its agreed budget. Report which effects introduced each variant so authors can consolidate redundant combinations.

Prefer a small set of intentional material profiles over arbitrary flag mixing. Precompile the most common variants during a loading transition, then lazily compile rare variants with telemetry.

The [NixieFX PixiJS particle workflow](https://nixiefx.com/pixijs-particle-effects/) is useful when designing an author-to-runtime boundary in which exported effects remain portable and the host retains control of asset loading.

## Validation

Test cold startup, scene transitions, context restoration, and low-end mobile GPUs. Record compile count, longest compile duration, and first-frame hitch. The goal is not zero variants; it is a bounded set that can be reasoned about and warmed predictably.
