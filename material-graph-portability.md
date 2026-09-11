# Review material graphs for renderer portability

A material graph that looks correct in a Three.js shader may need approximation when rendered through a batched 2D pipeline. Decide whether an effect is truly portable before content reaches integration.

Start with the target profile. Prefer particle color, relative time, UV transforms, baked gradients, and texture sampling when the effect must run in both PixiJS and Three.js. Reserve dynamic per-pixel features, prepared 3D meshes, and scene-lighting dependencies for Three.js-specific effects.

During review:

- inspect each backend support report;
- treat `blocked` as a failed build for the required backend;
- document intentional `partial` approximations;
- compare bloom and blend behavior in both previews;
- verify texture alpha and premultiplication assumptions.

Do not infer portability from a successful export alone. The support report is part of the artifact and should travel with the effect into CI.

The [NixieFX feature reference](https://nixiefx.com/vfx-runtime-docs/) explains backend-aware material compilation and portable target profiles.
