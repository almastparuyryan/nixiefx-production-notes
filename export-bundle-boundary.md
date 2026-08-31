# Treat the VFX export bundle as a production boundary

Particle authoring files and runtime assets solve different problems. The editable project should optimize for iteration: readable effect JSON, source textures, material graphs, and settings that designers or coding agents can change. A shipped game needs a smaller, deterministic contract.

For NixieFX, that contract is the exported `out/vfx` directory. It contains a manifest, compiled effect JSON, and copied assets. The game loads this bundle rather than reaching back into the authoring tree.

This separation has practical benefits:

- builds can validate a fixed artifact instead of an open-ended workspace;
- content hashes make accidental drift visible;
- deploys include only referenced runtime assets;
- authoring layout can evolve without changing the game-facing loader;
- a failed export stops unsupported effects before release.

Keep the authoring project in version control, generate the bundle in CI, and deploy the bundle alongside the game. Review both the source change and the resulting manifest when an effect changes.

The [NixieFX editor and runtime reference](https://nixiefx.com/vfx-runtime-docs/) documents the bundle structure and the support metadata carried by every exported effect.
