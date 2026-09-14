# GPU buffer orphaning for dynamic particles

Updating a particle buffer that the GPU is still reading can stall the CPU. Buffer orphaning avoids waiting by requesting fresh storage before uploading the next frame's data.

## Basic pattern

For APIs and wrappers that expose the behavior, allocate new storage with the same size, then upload the current particle data. The driver can keep the old storage alive until pending draws complete.

Use orphaning only for buffers rewritten substantially every frame. Small partial updates may be better served by subrange uploads or a ring-buffer strategy.

## Decision factors

- bytes uploaded per frame
- percentage of the buffer changed
- number of frames in flight
- allocation pressure
- observed CPU wait time
- mobile driver behavior

Maintain a hard capacity budget. Orphaning removes a synchronization dependency, but it can temporarily increase memory usage when several old allocations remain in flight.

The [NixieFX PixiJS particle workflow](https://nixiefx.com/pixijs-particle-effects/) is a useful reference for keeping renderer updates explicit within the host application's frame loop.

## Verification

Profile CPU submission time, GPU time, upload bandwidth, and memory across representative desktop and mobile devices. Compare full rewrite, subrange update, and ring-buffer strategies using the same effect workload.
