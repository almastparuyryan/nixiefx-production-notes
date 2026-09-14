# Texture Lifetime Policy for Browser VFX

Particle textures are frequently shared by several emitters, scenes, and pooled effects. Disposing them from the wrong owner causes missing particles; never disposing them causes GPU memory growth.

## Ownership model

Use one explicit owner for each uploaded texture:

- the asset cache owns shared textures;
- an effect instance borrows textures from the cache;
- a scene may release its references, but it does not dispose a shared texture directly;
- the cache disposes the texture only when its reference count reaches zero or the application shuts down.

Keep the decoded image, GPU texture, and effect metadata as separate resources. They can have different lifetimes.

## Release checklist

1. Stop emitters from spawning.
2. Let required particles finish or terminate them deliberately.
3. Remove renderer objects from the scene.
4. Release effect references.
5. Dispose geometries and instance buffers owned by the effect.
6. Decrement shared texture references.
7. Verify GPU memory returns to the expected baseline after a scene cycle.

Test repeated load/unload loops, not just a single transition. Add a debug view that shows texture keys, owners, and reference counts; it turns intermittent black-quad bugs into traceable ownership mistakes.

For exported effects, align this policy with the lifecycle described in the [NixieFX runtime docs](https://nixiefx.com/vfx-runtime-docs/).
