# JellyPanel v1.4.1 — Large-Playlist Stability

This cumulative stability release keeps the v1.4 standalone multi-server design and corrects the intermittent blank server/dashboard state seen while importing a 17,150-channel M3U source.

## Changes

- M3U source creation and refresh now start tracked background jobs.
- Channel import uses PostgreSQL temporary staging, bulk COPY, and atomic upsert reconciliation.
- The previously committed channel snapshot remains available until the new import completes.
- Only one queued/running refresh job is allowed per M3U source.
- The Live TV page reports queued, running, completed, and failed import states.
- Server registry loading is separated from M3U, EPG, library, and operations requests.
- Temporary endpoint failures preserve the last valid fleet state instead of showing zero servers.
- Downstream HTTP 401 responses are checked against `/auth/me` before the panel displays its login page.
- Control API startup initialization failures are written to container logs.
- Adds `manage.sh m3u-jobs` and includes M3U job history in diagnostics.
- Database schema: 19.

## Upgrade

```bash
cd /root
sha256sum -c galaxytv-jellyfin-v1.4.1.zip.sha256
unzip -o galaxytv-jellyfin-v1.4.1.zip
cd galaxytv-jellyfin-v1.4.1
sudo bash install.sh --mode upgrade
```

## Validation

The build passed Python, shell, YAML, browser JavaScript, migration, archive, and checksum validation. A mocked 20,000-channel import completed through one bulk COPY stage and 14 fixed SQL execute calls rather than one statement per channel.
