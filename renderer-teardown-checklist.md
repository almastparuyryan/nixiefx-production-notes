# Teardown particle renderers without leaking scene state

Effects commonly outlive a route or game scene because their animation loop, GPU resources, or event subscriptions remain referenced. Define teardown as part of the integration contract.

When a scene closes:

1. stop spawning new effects;
2. remove the renderer update callback from the host loop;
3. destroy or detach every active effect instance;
4. destroy renderer-owned groups, buffers, and materials;
5. release texture or mesh stores only when their final owner exits;
6. clear event listeners and asynchronous preload callbacks.

Make teardown idempotent so route changes, error recovery, and hot reload can call it safely more than once. In development, repeat scene entry and exit while watching effect counts, DOM listeners, heap growth, and WebGL resource diagnostics.

The [NixieFX Three.js runtime guide](https://nixiefx.com/threejs-runtime/) documents renderer ownership, effect removal, unmounting, and destruction for exported effects.
