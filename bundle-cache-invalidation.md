# Cache exported VFX bundles by content identity

Particle effects often ship through a CDN, so a stale manifest can point at new assets or a new manifest can reference old files. Avoid mutable cache keys for production bundles.

Publish each export under a versioned or content-addressed directory. Upload assets and compiled effects first, then publish the manifest last. The manifest should index every effect and asset path and carry source hashes that the loader can validate.

For service workers and long-lived browser caches:

- never overwrite immutable hashed files;
- use short caching for the release pointer;
- use long immutable caching for versioned bundle contents;
- keep the previous bundle available during rollout;
- treat a missing or mismatched asset as a deployment failure, not a soft visual warning.

Log the bundle identity with client error reports so a rendering issue can be tied to the exact content shipped.

The [NixieFX runtime documentation](https://nixiefx.com/vfx-runtime-docs/) details the exported `out/vfx` manifest, compiled effect files, assets, and source hashes.
