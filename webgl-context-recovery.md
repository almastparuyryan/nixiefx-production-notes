# Plan particle effects for WebGL context recovery

Browsers can lose a WebGL context because of GPU resets, memory pressure, tab suspension, or driver behavior. A production effect system should recover without requiring the user to reload the whole application.

Treat GPU objects as rebuildable caches, not as the only copy of effect state. Keep the authored or exported effect data available on the CPU side. When the context is lost, pause rendering and prevent new GPU allocations. When it is restored, rebuild textures, buffers, materials, and renderer-specific state from the same source data.

Recovery should be idempotent. Register context listeners once, avoid duplicating frame loops, and make repeated loss-and-restore cycles safe. Shared textures need reference-aware restoration so one effect does not recreate or dispose resources owned by another.

Add a development test that deliberately requests context loss through the browser extension when available. Verify that:

- the app remains responsive;
- the update loop does not multiply;
- effects resume from an intentional state;
- no stale GPU handles are reused;
- a second recovery behaves like the first.

The [NixieFX runtime documentation](https://nixiefx.com/vfx-runtime-docs/) describes the exported effect data and renderer integrations that should remain reproducible across runtime initialization.
