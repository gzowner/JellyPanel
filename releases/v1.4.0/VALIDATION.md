# JellyPanel v1.4.0 Validation Report

## Completed static validation

The final release pass included focused architecture and regression assertions in addition to compilation, shell, YAML, browser, and archive checks.

- Python syntax and bytecode compilation for Control API, worker, M3U gateway, Operations Agent, Library Agent, Media Gateway, and deployment payload.
- Shell syntax validation for all installer, upgrade, repair, management, diagnostics, and node-deployment scripts.
- YAML parsing for central and Library Node Compose files.
- Browser JavaScript syntax validation after extracting the inline application script.
- 414 unique HTML identifiers and 393 JavaScript identifier references with no missing references.
- Migration ordering from schema 2 through schema 18.
- Standalone Compose topology: bundled Jellyfin services are profile-gated and core services do not depend on them.
- Empty-registry and first-external-primary code-path review.
- Per-server M3U source and category assignment validation.
- Server-specific HMAC feed-token isolation and disabled legacy gateway-route review.
- Per-server Live TV tuner/provider state and error-isolation review.
- Standalone backup/restore review for restore points without a local Jellyfin archive.
- Host and Library Node resource-metric payload validation.
- Release ZIP CRC, Unix executable metadata, extraction, and 114 internal SHA-256 manifest checks.

## Runtime boundary

Docker was unavailable in the build environment. The release has not been executed here against a real PostgreSQL container, Jellyfin fleet, M3U provider, remote SSH host, or systemd node.

Complete a disposable runtime test before production, including fresh standalone installation, v1.3.3 upgrade, external-server registration, user import, Library Node deployment, library synchronization, per-server Live TV, and standalone backup/restore.
