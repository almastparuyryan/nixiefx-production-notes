# Put a hard budget on sub-emitter recursion

Birth, collision, and death sub-emitters can create rich effects, but an innocent-looking chain can multiply work exponentially. Treat every nested spawn as part of one shared budget.

Set limits for:

- maximum nesting depth;
- total child instances per root effect;
- particles spawned per frame;
- collision-triggered children per particle;
- total lifetime of the effect tree.

Reject cycles during authoring when possible, then enforce runtime guards anyway because content can change after validation. When a budget is exhausted, skip the child spawn deterministically and expose a diagnostic counter. Silent nondeterministic dropping makes bugs hard to reproduce.

Stress-test the worst case: short-lived particles that all collide and spawn looping children. Verify that teardown cancels pending descendants and releases their renderer resources.

The [NixieFX editor and runtime reference](https://nixiefx.com/vfx-runtime-docs/) describes sub-emitter events, spawn budgets, and recursion guards in exported effects.
