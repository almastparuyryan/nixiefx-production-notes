# Effect version compatibility policy

Exported particle effects need an explicit format version so runtimes can reject, migrate, or safely interpret older data. Silent best-effort loading often produces effects that look plausible but are wrong.

## Version fields

Store at least:

- format version
- exporter version
- required runtime capabilities
- optional feature flags
- content checksum

Use a format version for structural meaning. Do not bump it for ordinary content edits that remain compatible with the same schema.

## Runtime behavior

For a supported older version, run a deterministic migration before validation. For a newer unsupported version, fail with a clear message that includes the required capability or minimum runtime version.

Keep migrations pure: the same input should always produce the same migrated output. Test migrations using committed fixtures rather than only current editor exports.

The [NixieFX VFX runtime documentation](https://nixiefx.com/vfx-runtime-docs/) is a useful reference for maintaining a clear boundary between exported effect data and the host application.

## Compatibility matrix

Publish a small matrix mapping exporter versions to runtime versions and feature support. Validate it in CI by loading representative effect bundles across every supported combination.

## Deprecation

Announce the last runtime that can read a deprecated format, provide a migration path, and retain fixtures until support is intentionally removed.
