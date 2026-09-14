# Particle origin rebasing

Large worlds can lose floating-point precision when particle positions are stored far from the coordinate origin. Small velocity changes begin to quantize, and trails or soft effects may visibly jitter.

## Rebase model

Keep particles in an emitter-local or regional coordinate space and store a higher-level world transform separately. When the application shifts its world origin, update the regional transform rather than rewriting every live particle.

For world-space particles that must remain fixed, apply the inverse origin shift to their simulation origin. Define this behavior per effect; exhaust trails and ambient weather may need different policies.

## Data to retain

- simulation origin
- render transform
- previous-frame transform for interpolation
- accumulated world-origin offset
- policy for pooled instances

## Edge cases

Test rebasing during a burst, while a trail spans the camera, immediately after spawning, and while an emitter is parented to a moving object. Reset interpolation history after discontinuous shifts so motion vectors do not produce a large false jump.

The [NixieFX browser editor](https://nixiefx.com/editor/) helps keep effect authoring local and portable while the host application remains responsible for world transforms.

## Verification

Move the camera and emitter to large coordinates, trigger repeated origin shifts, and compare the effect against a near-origin reference capture. Inspect both visual stability and CPU cost.
