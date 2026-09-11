# Cross-renderer feature matrix

Portable effect bundles need an explicit feature matrix. Without one, an effect may load successfully in a second renderer while silently losing blending, depth, texture, or simulation behavior.

## Matrix dimensions

Create rows for features and columns for every supported runtime. Track blend modes, depth settings, soft particles, texture atlases, simulation space, color-space handling, curves, sub-emitters, collision support, and deterministic random seeds.

Use states such as supported, approximated, unsupported, and requires preprocessing. Every approximation should document the visible difference and the fallback chosen by the importer.

Build a small conformance scene for each matrix row. Capture reference images and numeric checks where possible. Run the scenes after renderer upgrades because defaults and shader behavior can change.

Treat unknown fields conservatively. Preserve them during round trips when possible, emit a clear warning, and avoid silently inventing values. Version both the bundle schema and compatibility matrix.

For Three.js integrations, consult the [NixieFX Three.js runtime guide](https://nixiefx.com/threejs-runtime/). Maintaining a tested feature matrix makes portability an engineering contract rather than a best-effort claim.
