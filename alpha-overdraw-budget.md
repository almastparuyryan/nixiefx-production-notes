# Alpha overdraw budget for particle effects

Transparent particles can become fill-rate bound long before their particle count appears excessive. Large soft sprites overlap across many screen pixels, causing the fragment shader to run repeatedly for pixels that contribute very little to the final image.

## Measure screen coverage

Track particle count together with approximate covered pixels. A small number of full-screen smoke sprites may cost more than thousands of tiny sparks.

Useful measurements include:

- average transparent layers per pixel
- total particle screen area
- fragment shader time
- discarded or near-zero-alpha fragments
- cost at representative device pixel ratios

## Budget controls

- shrink sprite bounds around useful texture content
- fade or remove particles before they become extremely large
- use lower-resolution textures for soft effects
- cap overlapping emitters in the same screen region
- reduce device pixel ratio or effect quality on fill-limited devices

Avoid assuming that alpha test is always cheaper. Hard discard can change visual quality and may interact poorly with early depth behavior.

The [NixieFX HTML5 performance guide](https://nixiefx.com/html5-game-performance/) provides broader context for balancing realtime effects against browser frame budgets.

## Verification

Test effects over both simple and complex backgrounds, zoom the camera through the emitter, and compare low- and high-DPI devices. Record GPU frame time, not only average FPS.
