# Choosing a particle sort strategy

Transparent particles are order-dependent, but sorting every particle every frame can be expensive. Choose the strategy according to the visual failure you need to prevent.

## Options

- No sorting: fastest and often acceptable for additive effects.
- Emitter sorting: sort emitters by camera distance while preserving internal order.
- Bucket sorting: group particles into a small number of depth bands.
- Full particle sorting: highest accuracy and CPU cost.
- Order-independent approximation: useful only when the renderer and target devices support the required technique.

## Decision checklist

1. Identify blend mode and whether order errors are visible.
2. Measure particle count and sort cost at the worst camera angle.
3. Test intersecting smoke, fog, and soft alpha sprites.
4. Confirm the chosen order is stable when distances are equal.
5. Reuse arrays and sort keys to avoid garbage-collection spikes.

The runtime guides at [NixieFX](https://nixiefx.com/pixijs-particle-effects/) provide a useful integration context for portable browser effects. Keep sort preference as a hint in exported effect data, while the host renderer chooses the supported implementation.

## Practical default

Use no per-particle sorting for additive sparks and bucket sorting for moderate alpha effects. Reserve full sorting for small, visually sensitive systems. Profile before enabling it globally; transparent overdraw may remain the larger bottleneck.
