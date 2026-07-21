# JellyPanel v1.4.4 Per-Server Live TV Connection Test

## Upgrade

```bash
cd /root
sha256sum -c galaxytv-jellyfin-v1.4.4.zip.sha256
unzip -o galaxytv-jellyfin-v1.4.4.zip
cd galaxytv-jellyfin-v1.4.4
sudo bash install.sh --mode upgrade
```

Press `Ctrl+F5` after the upgrade.

## Preconditions

- A Jellyfin server is online with a valid API key.
- At least one enabled M3U source is assigned to that server.
- The M3U gateway port is reachable from the Jellyfin server.

## Test

1. Open **Live TV & EPG**.
2. Select the Jellyfin server.
3. Click **Sync selected server**.
4. Confirm the status changes to **Connected**.
5. Verify a GalaxyTV M3U tuner in Jellyfin Dashboard → Live TV.
6. Verify the assigned XMLTV guide provider.
7. Run the synchronization again and confirm it does not create duplicates.
8. Click **Refresh selected guide**.

## Diagnostics

```bash
cd /opt/galaxytv-jellyfin
sudo ./scripts/compose.sh logs --tail=300 control-api m3u-gateway
sudo ./scripts/manage.sh servers
sudo ./scripts/manage.sh m3u-jobs
```
