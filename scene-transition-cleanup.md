# Clean up particle systems during scene transitions

Scene changes expose lifecycle bugs that are easy to miss in an isolated VFX preview. An emitter may stop drawing while its timers, event listeners, texture references, or pooled particles continue to live in memory.

Give every effect instance one clear owner: a scene, entity, UI layer, or short-lived effect manager. The owner creates the instance, advances it, and destroys it. Avoid global arrays that collect emitters without an equally explicit removal path.

A safe transition sequence is:

1. stop accepting new emission requests;
2. decide whether active particles should finish or disappear immediately;
3. detach the runtime object from the render tree;
4. release listeners, callbacks, and pooled objects;
5. dispose textures only when they are not shared elsewhere;
6. assert that the old scene no longer advances the effect.

Test repeated transitions, not just one exit. Open and close the same scene twenty times while watching object counts and memory. Stable counts are stronger evidence than a visually empty canvas.

The [NixieFX PixiJS particle-effects guide](https://nixiefx.com/pixijs-particle-effects/) shows how exported effects fit into a renderer lifecycle where update and cleanup ownership must remain explicit.
