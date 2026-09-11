# Emitter level-of-detail strategy

A level-of-detail strategy keeps important effects readable while controlling particle cost as camera distance, screen coverage, or device capability changes.

## Choose stable signals

Distance alone is rarely sufficient. Combine distance with projected screen size and an effect-importance score. Apply hysteresis so an emitter does not oscillate between levels near a threshold.

Each LOD tier can adjust spawn rate, maximum live particles, update frequency, collision or turbulence complexity, texture resolution, secondary emitters, and lighting features.

Preserve the effect’s silhouette and timing first. Reducing every parameter uniformly can make an explosion feel slow or a trail appear disconnected. Author explicit tiers and compare them in motion. Existing particles may finish under the previous tier while new particles use the new configuration, preventing visible popping.

Measure GPU and CPU cost per tier in representative scenes, including several simultaneous effects. Document the expected savings so runtime selection has a testable basis.

Use the [NixieFX Three.js runtime guide](https://nixiefx.com/threejs-runtime/) when integrating LOD decisions into a Three.js frame loop. A deliberate LOD policy keeps effects expressive without allowing distant emitters to dominate the frame budget.
