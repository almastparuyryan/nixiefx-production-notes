# Validate and export VFX projects in CI

Visual authoring does not have to make particle assets opaque to automation. When effects are stored as JSON and the runtime bundle has a deterministic export step, CI can check the same project that artists and coding agents edit.

A small pipeline is enough:

1. install the pinned `nixie-fx` package on Node 20 or newer;
2. run `nixie-fx validate` against the project folder;
3. stop on invalid asset references or unsupported target features;
4. run `nixie-fx export` to generate `out/vfx`;
5. archive or deploy the manifest, compiled effects, and referenced assets;
6. exercise the game loader with the generated bundle.

Keep validation read-only and export deterministic. Avoid manually editing generated files after export; fix the authoring source and regenerate instead. If the bundle is committed, review manifest and effect diffs. If it is build-only, retain it as a CI artifact so a failed release can be reproduced.

This turns particle delivery into the same evidence-based process used for code: source, validation, build artifact, and runtime check.

The [NixieFX CLI reference](https://nixiefx.com/cli-reference/) documents project layout and the `effect create`, `validate`, and `export` commands for headless workflows.
