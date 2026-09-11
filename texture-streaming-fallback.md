# Texture streaming fallback for effects

Effects often depend on texture atlases that may not be resident when an emitter starts. A fallback policy prevents missing assets from producing invisible particles, broken materials, or frame stalls.

## Load states

Model texture availability explicitly as requested, fallback ready, full-resolution ready, or failed with a durable error. Create the material with a small neutral fallback texture so shader bindings remain valid. When the final texture arrives, swap it at a frame boundary and retain compatible sampling settings.

Do not let every emitter trigger its own download. Use a shared cache keyed by a versioned asset identifier and deduplicate concurrent requests. Apply memory-aware eviction without removing textures referenced by live effects.

For failures, log the asset ID, resolved URL, response status, and affected effect. Retry only transient errors with bounded backoff. Permanent failures should select a designed fallback and keep the scene running.

The [NixieFX feature and runtime documentation](https://nixiefx.com/vfx-runtime-docs/) is a useful reference when connecting exported effects to asset pipelines. A clear streaming fallback keeps effects visible and diagnosable under slow networks and constrained memory.
