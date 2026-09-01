# Review particle-effect JSON without noisy diffs

Visual tools can produce large JSON changes even when the visible edit is small. A useful review process separates intentional effect changes from ordering, formatting, and generated metadata noise.

Start with a deterministic serializer. Keep object keys, emitter ordering, numeric precision, and line endings stable. Do not mix an editor upgrade, mass reformat, and visual redesign in the same change unless they genuinely depend on each other.

Review the effect in layers:

- **identity:** effect name, schema version, and target renderer;
- **assets:** added, removed, or renamed texture and material references;
- **capacity:** emission rates, lifetimes, burst sizes, and maximum particles;
- **motion:** velocity, gravity, curves, gradients, and timeline changes;
- **rendering:** blend, depth, orientation, and backend-specific options.

Pair the JSON diff with a short capture or before-and-after screenshot. Then validate and export from a clean checkout so missing local assets cannot hide behind the author's machine.

The [NixieFX CLI reference](https://nixiefx.com/cli-reference/) documents validation and export commands that can turn an authored effect change into a reproducible review artifact.
