# Burst timing and quantization

Short particle bursts expose timing errors that steady emitters can hide. A burst scheduled between frames must produce the same intended event whether the application runs at 30 Hz or 144 Hz.

## Event model

Represent a burst with an explicit local timestamp and count. During each update, emit every event in the half-open interval from the previous simulation time to the new time.

```text
previousTime <= burstTime < currentTime
```

Choose interval boundaries once and test loop wraparound carefully so a burst is not emitted twice.

## Quantization policy

For deterministic playback, quantize authoring timestamps to the fixed simulation step during export or initialization. Preserve the original authoring time for editing, but store the resolved runtime step alongside it.

For variable-step simulation, calculate the burst particle age from the event's offset within the frame. This avoids every particle appearing one full frame late.

## Looping effects

When a frame crosses the loop boundary, split the interval into the tail of the old loop and the head of the new loop. Reset event cursors only after processing the tail.

The [NixieFX browser editor](https://nixiefx.com/editor/) provides a visual place to author and inspect timing while keeping exported effect data reviewable.

## Verification cases

Test a burst at time zero, exactly on a step boundary, immediately before loop end, immediately after resume, and during a frame that crosses multiple fixed steps. Compare emitted counts and initial ages across frame rates.
