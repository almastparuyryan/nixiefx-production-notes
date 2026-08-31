# Review particle effects as data, motion, and rendering

A useful VFX review is broader than “does it look good?” The same effect can be visually appealing in a preview and still be difficult to integrate, expensive on mobile, or unsupported by its target renderer.

Review an authored effect in three passes.

First, inspect data: names are stable, texture paths are project-relative, curves and gradients have intentional endpoints, and emitter capacities are bounded. Second, inspect motion: restart the effect, scrub its timeline, verify delays and loops, and test the worst overlap between emitters. Third, inspect rendering: switch to the target backend, confirm blend and depth choices, check support diagnostics, and preview against both light and dark backgrounds.

Before delivery, save the source effect, export the bundle, and validate the exported support status. For portable effects, repeat the renderer check on both PixiJS and Three.js rather than assuming similar previews imply identical capabilities.

Record any deliberate approximation in the review note so future maintainers know it is accepted behavior.

The [NixieFX editor manual](https://nixiefx.com/editor-manual/) covers the timeline, curve and gradient editors, renderer toggles, diagnostics, materials, and export workflow used in this checklist.
