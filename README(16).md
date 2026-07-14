# GalaxyTV Jellyfin v1.0.0 Stable

GalaxyTV is a cumulative Docker-based management layer around stock Jellyfin. This stable archive contains every feature tested through v0.11.0 and replaces the sequence of development ZIP files.

The release includes:

- Stock Jellyfin 10.11.11 on a configurable host port.
- GalaxyTV administrator and reseller control panel.
- User expiration and package/session policy synchronization.
- Packages, resellers, sub-resellers, and credit ledger.
- M3U source import, filtering, and private gateway.
- XMLTV EPG import, multi-feed priority, and automatic EPG mapper.
- Fast rclone-aware media inventory without ffprobe.
- Movies and TV Shows STRM/NFO mirror with TMDB metadata.
- Internal media gateway for stable STRM playback paths.
- Operations Center, service controls, managed backups, and restores.
- Stream Operations with current playback, warnings, messaging, stop, and suspend controls.
- Stable installer modes for fresh install, upgrade, repair, restore, diagnostics, and data-preserving uninstall.

HTTPS remains optional and is not enabled by the default local installation.

## Supported server

The v1.0.0 installer is validated for a clean **Ubuntu Server 24.04 LTS** installation using `amd64` or `arm64`.

Recommended minimum:

- 4 GB RAM.
- 2 CPU cores.
- 10 GB free application storage, excluding media and rclone cache.
- Docker-compatible kernel.
- A mounted media location such as `/mnt/unionfs`.

The installer refuses other operating systems unless `--allow-unsupported` is provided.

## Fresh installation

Copy the ZIP to the server, then run:

```bash
sudo apt-get update
sudo apt-get install -y unzip

unzip galaxytv-jellyfin-v1.0.0.zip
cd galaxytv-jellyfin-v1.0.0
sudo bash install.sh
```

Select:

```text
1) Fresh installation
```

The guided installer asks for:

- Media mount path.
- Media scan root.
- Rclone remote and config directory.
- Timezone.
- Panel administrator username and password.
- Jellyfin and GalaxyTV ports.
- Optional `/dev/dri` GPU access when detected.

Defaults retain the tested layout:

```text
GalaxyTV panel:       8181
Control API:          8182, localhost only
M3U gateway:          8183, localhost only
Jellyfin:             8097
Media mount:          /mnt/unionfs
Media inventory root: /mnt/unionfs/Media
Rclone remote:        union:
Rclone config:        /root/.config/rclone
Install directory:    /opt/galaxytv-jellyfin
```

Emby can continue using port `8096`.

### Non-interactive installation

```bash
export GALAXYTV_PANEL_PASSWORD='replace-with-a-strong-password'

sudo -E bash install.sh \
  --mode fresh \
  --non-interactive \
  --media-path /mnt/unionfs \
  --scan-root /mnt/unionfs/Media \
  --rclone-remote union: \
  --rclone-config-dir /root/.config/rclone \
  --timezone America/Chicago \
  --panel-user admin \
  --jellyfin-port 8097 \
  --panel-port 8181 \
  --api-port 8182 \
  --m3u-port 8183
```

Do not place production passwords directly in shell history when an environment variable can be used.

## Upgrade an existing GalaxyTV installation

The same `install.sh` detects `/opt/galaxytv-jellyfin/.env`. Select **Upgrade existing installation**, or run:

```bash
cd galaxytv-jellyfin-v1.0.0
sudo bash install.sh --mode upgrade
```

The upgrade process:

1. Checks Ubuntu, Docker, disk space, and dependencies.
2. Creates a local application and PostgreSQL rollback snapshot.
3. Requests a managed pre-upgrade backup when the Operations Agent is available.
4. Preserves `.env`, Jellyfin, PostgreSQL, Redis, backups, logs, STRM output, credentials, and media.
5. Installs all v1.0.0 application files.
6. Applies additive database migrations.
7. Pulls/builds containers and starts the stack.
8. Verifies every critical endpoint and database schema.
9. Automatically restores the previous application files if validation fails.

Rollback snapshots are stored under:

```text
/opt/galaxytv-jellyfin/backups/upgrades/
```

Manual application rollback:

```bash
sudo /opt/galaxytv-jellyfin/scripts/rollback-last-upgrade.sh
```

Database migrations are additive and are intentionally retained during an application-file rollback.

## Installer modes

Run `sudo bash install.sh` for the menu, or specify a mode:

```bash
sudo bash install.sh --mode fresh
sudo bash install.sh --mode upgrade
sudo bash install.sh --mode repair
sudo bash install.sh --mode restore
sudo bash install.sh --mode diagnostics
sudo bash install.sh --mode uninstall
```

### Repair

Repair re-copies the release files when run from the extracted ZIP, restores missing environment defaults, recreates directories, rebuilds containers, and runs complete health validation.

```bash
sudo bash install.sh --mode repair
```

From an installed system:

```bash
sudo /opt/galaxytv-jellyfin/scripts/manage.sh repair
```

Running repair from the installed directory can rebuild services, but an extracted v1.0.0 ZIP is preferred when application files may be damaged.

### Restore

Managed restore points are stored in:

```text
/opt/galaxytv-jellyfin/backups/managed
```

Run:

```bash
sudo bash install.sh --mode restore
```

The installer lists completed restore points and asks which one to apply. For non-interactive restore:

```bash
sudo bash install.sh --mode restore --backup-id BACKUP_UUID --non-interactive --yes
```

To move a restore point from another server, copy its entire backup directory into `backups/managed` before starting restore.

A restore point without a Jellyfin archive can still restore GalaxyTV PostgreSQL and optional STRM data; the job reports that Jellyfin was skipped.

### Diagnostics

```bash
sudo bash install.sh --mode diagnostics
```

Or:

```bash
sudo /opt/galaxytv-jellyfin/scripts/manage.sh diagnostics
```

The resulting redacted ZIP is stored in:

```text
/opt/galaxytv-jellyfin/backups/diagnostics
```

Review diagnostics before sharing. The exporter redacts recognized passwords, keys, tokens, and Xtream-style credentials, but no automatic redaction system is perfect.

### Data-preserving uninstall

```bash
sudo bash install.sh --mode uninstall
```

This removes GalaxyTV containers and networks without deleting bind-mounted data. It does not remove Docker, media, Jellyfin configuration, PostgreSQL, Redis, backups, or STRM output.

Restore services later with repair mode.

## Management commands

```bash
sudo /opt/galaxytv-jellyfin/scripts/manage.sh status
sudo /opt/galaxytv-jellyfin/scripts/manage.sh health
sudo /opt/galaxytv-jellyfin/scripts/manage.sh logs
sudo /opt/galaxytv-jellyfin/scripts/manage.sh backup
sudo /opt/galaxytv-jellyfin/scripts/manage.sh diagnostics
sudo /opt/galaxytv-jellyfin/scripts/manage.sh streams
sudo /opt/galaxytv-jellyfin/scripts/manage.sh credentials
sudo /opt/galaxytv-jellyfin/scripts/manage.sh panel-password
sudo /opt/galaxytv-jellyfin/scripts/manage.sh jellyfin-key
```

Complete health check:

```bash
sudo /opt/galaxytv-jellyfin/scripts/health-check.sh
```

## First server setup

After installation:

1. Open the printed Jellyfin URL and complete Jellyfin's normal setup wizard.
2. Create an API key in Jellyfin under **Dashboard → Advanced → API Keys**.
3. Save it with:

   ```bash
   sudo /opt/galaxytv-jellyfin/scripts/set-jellyfin-api-key.sh
   ```

4. Open the GalaxyTV panel.
5. Configure M3U and XMLTV feeds.
6. Run the EPG Mapper and review mappings before applying them.
7. Run a media inventory scan.
8. Configure the Library Mirror and TMDB credential.
9. Build the mirror.
10. Add Jellyfin libraries:

   ```text
   Movies:   /strm-library/Movies
   TV Shows: /strm-library/TV Shows
   ```

11. Create a managed backup from the Operations page.

Disable or remove old Jellyfin libraries that scan the cloud mount directly after the STRM libraries have been verified. Otherwise Jellyfin will continue performing the old slow scans.

## Hardware acceleration

When `/dev/dri` exists, the installer can enable device access for Jellyfin. This only passes the device into the container; hardware acceleration must still be configured in Jellyfin.

The setting is stored as:

```text
ENABLE_INTEL_GPU=true
```

The same compose wrapper is used by install, repair, upgrades, and normal management so the device mapping is retained.

## Persistent data

The following locations survive upgrades, repairs, and data-preserving uninstall:

```text
/opt/galaxytv-jellyfin/.env
/opt/galaxytv-jellyfin/jellyfin/config
/opt/galaxytv-jellyfin/jellyfin/cache
/opt/galaxytv-jellyfin/postgres/data
/opt/galaxytv-jellyfin/redis/data
/opt/galaxytv-jellyfin/backups
/opt/galaxytv-jellyfin/logs
/opt/galaxytv-jellyfin/strm-library
```

The media mount itself is never deleted or backed up by the installer. Media bind mounts use read-only `rslave` propagation so a host rclone/NFS mount can become visible after container creation. Restart the GalaxyTV stack after changing the host mount configuration.

## Security notes

- The Control API and M3U gateway bind to `127.0.0.1` by default.
- The Operations Agent and media gateway are Docker-internal only.
- Panel cookies are non-secure for local HTTP operation.
- Do not expose the panel or Jellyfin directly to the public internet without HTTPS, firewall rules, and a correctly configured reverse proxy.
- Keep `.env`, credentials, backups, and diagnostics private.
- Change generated credentials before sharing a cloned server image.

See [SECURITY.md](SECURITY.md) before public deployment.

## Release validation

The ZIP contains `release-files.sha256`. Fresh, upgrade, and repair modes verify the extracted release files before changing the server.

The build was validated with:

- Bash syntax checks for every shell script.
- Python bytecode compilation for all Python services.
- Docker Compose YAML parsing.
- Inline control-panel JavaScript syntax validation.
- Release checksum verification.
- ZIP integrity testing.

Docker was not available in the build environment, so the final Docker runtime and clean Ubuntu 24.04 installation must be confirmed on the real test server.
