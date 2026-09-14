# Shader Compile Warmup for Particle Effects

The first visible use of a particle material can stutter if shader compilation happens inside a live frame. A warmup pass moves that cost to a controlled point in the loading sequence.

## Practical sequence

1. Build the renderer with the same color space, tone mapping, precision, and feature flags used in gameplay.
2. Create representative particle materials for each meaningful variant: blending mode, texture path, lighting option, and renderer integration.
3. Place tiny or hidden emitters in a disposable warmup scene.
4. Compile or render one controlled frame after textures are ready.
5. Wait for the GPU work to settle before the transition into interactive content.
6. Dispose only the disposable scene objects; keep reusable materials and textures alive when the next scene needs them.

## Boundaries

Do not generate every theoretical define combination. Warm the variants that shipping content actually uses. Record compile time separately from steady-state frame time, because combining them hides the source of the hitch.

Test cold starts on representative mobile hardware. Desktop development machines often mask compilation spikes. If the app supports renderer recreation after a WebGL context loss, repeat the warmup on the rebuilt renderer.

NixieFX's [VFX runtime documentation](https://nixiefx.com/vfx-runtime-docs/) is the natural reference point when mapping exported effect assets to the runtime materials that need warming.
