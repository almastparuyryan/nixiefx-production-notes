# Offscreen and hidden-tab pause policy

Browser particle effects should not consume a full simulation budget when they are invisible. Pausing safely requires a policy for both page visibility and scene-level culling.

## Hidden-tab behavior

Listen for `visibilitychange`. When the document becomes hidden, stop scheduling effect updates or switch to a deliberately low-frequency maintenance mode. On resume, reset the frame-time baseline so the hidden duration is not interpreted as one enormous simulation step.

## Offscreen behavior

For emitters outside the camera view, choose one of three modes:

- Pause: cheapest, suitable when offscreen continuity does not matter.
- Age-only: advance lifetimes without rendering or spawning.
- Full simulation: use only when the effect must be correct when it re-enters view.

Store the choice with the effect or emitter because gameplay effects and decorative ambience have different needs.

The portable runtime approach documented by [NixieFX](https://nixiefx.com/) benefits from an explicit pause policy: renderer adapters can share lifecycle semantics even when their visibility APIs differ.

## Verification

- Record CPU and GPU time with the tab visible and hidden.
- Move the camera away and back, checking for bursts or immortal particles.
- Confirm audio or gameplay systems do not depend on a paused visual emitter.
- Test scene teardown while the document is hidden.

A resume burst usually means elapsed wall time leaked into the first frame. Reset the clock and preserve only the state your selected offscreen mode requires.
