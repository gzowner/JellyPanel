# JellyPanel v1.4.0 — Standalone Multi-Server Control Plane

JellyPanel v1.4.0 changes the default installation from “panel plus bundled Jellyfin” to a true standalone management panel. A fresh installation can start with no Jellyfin container at all. Register any existing Jellyfin server from the Servers page; the first server becomes the primary account server.

This is a cumulative release. It contains the user, package, reseller, session, M3U/XMLTV, EPG mapping, STRM library, remote-node, backup, diagnostics, and multi-server features from earlier releases.

## Major changes

### Standalone by default

- Fresh installs do not start or expose a Jellyfin container.
- The core stack is PostgreSQL, Redis, Operations Agent, Control API, worker, M3U gateway, and web panel.
- The Servers page works when the registry is empty.
- The first external Jellyfin server becomes the primary server automatically.
- An optional panel-managed local Jellyfin profile remains available with `--with-local-jellyfin`.
- Existing upgrades preserve their current local-profile setting unless explicitly changed.

### Productive fleet dashboard

The dashboard now shows:

- Registered and online server counts.
- Combined active playback sessions.
- Panel service health.
- Host CPU usage and load.
- Host memory usage.
- Panel disk availability.
- Network receive/transmit totals and interface state.
- Per-server API health and Jellyfin version.
- Per-server Library Node health.
- Remote CPU, memory, source/output free space, network, and active streams when a Library Node is installed.

### Independent server control

Each registered Jellyfin server is treated as a separate target:

- Test, edit, enable, disable, and remove servers independently.
- Import and link existing users per server.
- Create or synchronize user assignments across selected servers.
- Monitor sessions across the whole fleet while preserving the originating server.
- Install a Library Node on each remote server using Docker Compose or native Python/systemd.
- Configure separate source paths, STRM destinations, schedules, and missing-file rules per server.

### Per-server M3U and XMLTV

- Assign every M3U source to one or more selected Jellyfin servers.
- Enable a different category set for each assigned server.
- Each server receives a unique secured playlist URL containing only its assigned sources.
- XMLTV proxy feeds are generated for that server’s assigned sources.
- Connect or refresh Live TV independently for each server.
- Existing sources are assigned to all existing registered servers during upgrade to preserve previous behavior.
- New sources use explicit server assignment from the source editor.

The M3U gateway port must be reachable from the registered Jellyfin servers. Restrict it by firewall to those server IP addresses. Each feed path contains a server-specific HMAC token derived from the panel master gateway secret. Treat the complete URL as a credential; one server token cannot be reused with another server UUID.

### Navigation cleanup

The main navigation now focuses on active workflows:

- Dashboard
- Users
- Packages
- Resellers
- Servers
- Libraries
- Live TV & EPG
- EPG Mapper
- Stream Operations
- Operations
- Audit

Legacy central media-scan and mirror routes remain in the codebase for upgrade compatibility but are no longer primary navigation items. New library work should use the per-server Libraries page and Library Nodes.

## Requirements

Central panel:

- Ubuntu 24.04 recommended.
- Root or sudo access.
- Docker Engine and Docker Compose plugin; the installer can install them.
- Panel port `8181` by default.
- Control API port `8182`, bound to localhost only.
- M3U gateway port `8183`, reachable by remote Jellyfin servers when Live TV is used.

Registered Jellyfin servers:

- Jellyfin 10.10 or 10.11 recommended.
- Administrator-created API key.
- The central Control API container must be able to reach the server’s internal URL.
- Library Node required for remote disk, CPU, memory, network, and per-server STRM operations.

## Fresh standalone installation

```bash
cd /root
sha256sum -c galaxytv-jellyfin-v1.4.0.zip.sha256
unzip -o galaxytv-jellyfin-v1.4.0.zip
cd galaxytv-jellyfin-v1.4.0
sudo bash install.sh --standalone-only
```

Interactive `sudo bash install.sh` also defaults to no local Jellyfin when the optional-local-server question is answered **No**.

After installation:

```bash
sudo /opt/galaxytv-jellyfin/scripts/manage.sh health
sudo cat /opt/galaxytv-jellyfin/panel-admin-credentials.txt
```

Open `http://SERVER_IP:8181`, sign in, then open **Servers → Add Server**.

Use an internal URL reachable from inside the `control-api` container. For a Jellyfin server on another host, do not use `127.0.0.1`.

## Optional local Jellyfin profile

A new installation can still include a panel-managed Jellyfin Docker server:

```bash
sudo bash install.sh --with-local-jellyfin
```

Default optional local Jellyfin port: `8097`.

This profile additionally starts:

- `galaxytv-jellyfin`
- `galaxytv-library-agent`
- `galaxytv-media-gateway`

Standalone installs do not start these services.

## Upgrade from v1.3.3

```bash
cd /root
sha256sum -c galaxytv-jellyfin-v1.4.0.zip.sha256
unzip -o galaxytv-jellyfin-v1.4.0.zip
cd galaxytv-jellyfin-v1.4.0
sudo bash install.sh --mode upgrade
```

The upgrade preserves the existing `MANAGED_LOCAL_JELLYFIN` setting. An existing installation that already uses the bundled local Jellyfin continues to use it.

Do not switch an established installation with managed users from the local primary server to standalone-only without planning a user-ID migration. User identities are anchored to the current primary server.

## Register the first external server

1. Open **Servers**.
2. Select **Add Server**.
3. Enter a unique name.
4. Enter an internal URL reachable from the panel containers.
5. Enter the public/user-facing URL.
6. Enter a Jellyfin API key.
7. Save.

When no servers exist, this server becomes primary automatically. Before managed users exist, another server can be promoted with **Make primary**.

## Per-server Library Node

Open **Libraries → Install Node** and choose:

- Docker Compose, or
- Native Python + systemd.

The node reports health and system metrics and performs local STRM scanning without exposing SSH credentials after deployment. SSH passwords, keys, passphrases, and sudo passwords are used only for the active installation request and are not stored.

Recommended workflow:

1. Install or connect the node.
2. Test the agent.
3. Configure the server’s source and output paths.
4. Run **Adopt Existing** when STRM files already exist.
5. Run **Verify Existing**.
6. Run **Sync Changes** with a small source folder.
7. Add the resulting Movies and TV Shows folders to that Jellyfin server.
8. Enable daily synchronization after verification.

## Per-server Live TV

1. Open **Live TV & EPG**.
2. Add or edit an M3U source.
3. Select the Jellyfin servers that should receive it.
4. Refresh the source.
5. Choose a server from the page selector.
6. Open **Categories** and select the category set for that server.
7. Select **Connect / update Live TV**.
8. Refresh the guide.

The server-specific playlist format is:

```text
http://PANEL_IP:8183/feed/SERVER_TOKEN/server/SERVER_UUID.m3u
```

The panel writes this URL to Jellyfin through the API; it is not necessary to paste it manually during normal operation.

The old unprotected `/playlist/master.m3u`, `/epg/...`, and `/stream/...` routes are disabled in v1.4.0 because port 8183 may be reachable by remote servers. After upgrading, synchronize Live TV once for each server so Jellyfin receives its secured feed URL.

## Backups

Standalone backups include:

- PostgreSQL.
- Panel configuration snapshot.
- Per-server Library Node inventory stored by the central panel.
- Optional central STRM workspace.

A managed local Jellyfin archive is included only when the optional local profile is enabled. Restore points without a local Jellyfin archive remain valid and can restore the standalone control plane.

## Useful commands

```bash
sudo /opt/galaxytv-jellyfin/scripts/manage.sh status
sudo /opt/galaxytv-jellyfin/scripts/manage.sh health
sudo /opt/galaxytv-jellyfin/scripts/manage.sh servers
sudo /opt/galaxytv-jellyfin/scripts/manage.sh libraries
sudo /opt/galaxytv-jellyfin/scripts/manage.sh deployments
sudo /opt/galaxytv-jellyfin/scripts/manage.sh streams
sudo /opt/galaxytv-jellyfin/scripts/manage.sh diagnostics
```

## Database

v1.4.0 uses schema **18**.

Migration 018 adds per-server M3U assignments and per-server Jellyfin Live TV connection state. It does not delete existing users, servers, packages, resellers, credits, source data, EPG mappings, STRM inventory, or backup records.

## Validation scope

The release is statically validated for Python, shell, YAML, browser JavaScript, HTML identifiers, migration ordering, standalone Compose topology, primary-server behavior, per-server M3U assignment, secured gateway routing, system-metric payloads, and archive integrity.

Docker is unavailable in the build environment, so the final runtime validation must be completed on a test host before production deployment.

No media, channels, accounts, or content sources are included. Use only media and sources you own or are authorized to manage.
