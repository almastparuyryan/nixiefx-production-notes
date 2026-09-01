# Define a particle pooling policy before optimizing

Pooling can remove allocation spikes, but an unbounded pool simply converts short-lived allocations into permanent memory. Define the policy before implementing the container.

Start with evidence from a representative scene. Record simultaneous effects, peak live particles, object creation time, garbage-collection pauses, and memory after the scene becomes idle. Pool only objects whose allocation or initialization is measurable in the frame budget.

A practical policy specifies:

- the warm capacity available before gameplay starts;
- the maximum retained capacity after a burst;
- how long excess objects remain idle before release;
- which fields are reset when an object returns;
- whether textures and materials are shared or owned;
- what happens when demand exceeds the cap.

Reset every mutable field, including callbacks and parent references. A pooled particle carrying state from its previous use can create visual bugs that appear random. Add a soak test with repeated bursts and confirm that retained memory returns to the defined ceiling.

Pooling is most valuable when combined with a bounded emitter budget and stable frame loop. The [NixieFX Three.js game tutorial](https://nixiefx.com/threejs-game-tutorial/) provides a practical integration context for exported effects in a mobile-conscious game loop.
