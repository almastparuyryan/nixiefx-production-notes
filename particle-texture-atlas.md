# Prepare particle textures for predictable WebGL rendering

Particle textures are small assets with an outsized effect on visual quality and runtime cost. A production-ready texture should have deliberate alpha, enough padding to survive filtering, and dimensions that match how it will be sampled.

Use premultiplied-alpha assets consistently. Mixing premultiplied and straight-alpha textures often creates dark or bright fringes around soft particles. Add transparent padding around atlas regions so linear filtering does not pull color from a neighboring sprite. If mipmaps are enabled, inspect the smallest levels as well as the full-resolution preview.

Keep the source texture and its export settings together. Record color space, compression, wrap mode, filtering, and intended blend mode. Two files that look identical in an image editor can behave very differently after GPU upload.

Before shipping:

- preview the effect over light and dark backgrounds;
- test at the smallest expected on-screen size;
- check the atlas at non-integer scales;
- verify texture paths after bundling;
- confirm the target renderer uses the intended alpha convention.

The [NixieFX editor manual](https://nixiefx.com/editor-manual/) explains the material, texture, preview, and export controls used when preparing particle assets.
