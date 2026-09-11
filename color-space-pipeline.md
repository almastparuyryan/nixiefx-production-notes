# Particle color-space pipeline

Washed-out textures, overly bright additive particles, and mismatched editor/runtime previews often come from an unclear color-space pipeline. Decide where decoding and encoding happen, then make every renderer adapter follow the same contract.

## Recommended contract

1. Treat color textures authored for display as sRGB inputs.
2. Decode to linear space before lighting, tint multiplication, and interpolation.
3. Perform additive or alpha blending in the renderer's intended working space.
4. Encode once for the output surface.
5. Treat data textures such as masks and noise as linear unless their specification says otherwise.

## Review checklist

- Texture metadata identifies color textures separately from scalar data.
- Hex UI colors are converted consistently before shader use.
- Premultiplied-alpha expectations match texture import and blend state.
- Tone mapping is applied once, not in both the effect shader and renderer.
- Screenshots compare editor and runtime under the same output settings.

When exporting effects from [NixieFX](https://nixiefx.com/editor/), keep color intent in the effect metadata rather than relying on one engine's defaults. Renderer adapters can then map that intent to the current Three.js or PixiJS color-management API.

## Debug method

Test a neutral gray texture, a saturated tint, and an additive gradient. Disable tone mapping temporarily. If the neutral sample changes between editor and runtime, inspect texture decoding; if only additive effects differ, inspect blend and output encoding.
