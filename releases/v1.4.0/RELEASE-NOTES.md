# JellyPanel v1.4.0 — Standalone Multi-Server Control Plane

## Highlights

- Fresh installations are standalone by default and do not install or start Jellyfin.
- An existing Jellyfin server can be registered immediately; the first registered server becomes primary.
- The bundled local Jellyfin Docker profile remains optional through `--with-local-jellyfin`.
- New fleet dashboard shows central service health, CPU, memory, disk, network, active sessions, and per-server health.
- Library Nodes report remote CPU, memory, source/output free space, network, and STRM job status.
- Each registered Jellyfin server is managed independently from the same panel.
- M3U sources can be assigned to selected servers.
- Category selections can differ per server even when using the same M3U source.
- Each server receives a server-specific HMAC-protected M3U/XMLTV feed.
- Live TV connection and guide refresh run independently per server.
- Old unprotected M3U gateway routes are disabled.
- Navigation is simplified around Dashboard, Users, Packages, Resellers, Servers, Libraries, Live TV & EPG, EPG Mapper, Stream Operations, Operations, and Audit.
- Standalone backups and restores no longer require a local Jellyfin archive.
- Database schema is 18.

## Fresh standalone install

```bash
cd /root
sha256sum -c galaxytv-jellyfin-v1.4.0.zip.sha256
unzip -o galaxytv-jellyfin-v1.4.0.zip
cd galaxytv-jellyfin-v1.4.0
sudo bash install.sh --standalone-only
```

## Upgrade from v1.3.3

```bash
cd /root
sha256sum -c galaxytv-jellyfin-v1.4.0.zip.sha256
unzip -o galaxytv-jellyfin-v1.4.0.zip
cd galaxytv-jellyfin-v1.4.0
sudo bash install.sh --mode upgrade
```

Existing upgrades preserve their current managed-local-Jellyfin setting. After upgrading, synchronize Live TV once for every server so Jellyfin receives its new secured server-specific feed URL.

## Runtime validation boundary

The release passed Python, shell, YAML, browser JavaScript, HTML reference, migration-order, architecture, and archive-integrity checks. Docker was unavailable in the build environment, so a disposable runtime test is required before production deployment.
