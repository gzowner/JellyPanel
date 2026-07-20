# JellyPanel v1.4.1 Large M3U Stability Test

## Goal

Confirm that importing a large M3U source does not hide registered servers, clear the dashboard, or force the administrator back to the login page.

## Test

1. Upgrade from v1.4.0 and confirm `manage.sh health` reports schema 19.
2. Register at least one Jellyfin server and verify it is visible on Dashboard and Servers.
3. Add an M3U source containing at least 10,000 channels.
4. Confirm the Add Source dialog closes immediately and the source shows `queued` or `running`.
5. Navigate repeatedly between Dashboard, Servers, Libraries, and Live TV while the import runs.
6. Confirm the registered server remains visible and the panel login does not reappear.
7. Confirm the M3U source progress changes from staging/reconciling to completed.
8. Confirm the final channel/category counts are populated and category selection opens.
9. Press Refresh repeatedly while a job is running and confirm only one active job remains.
10. Temporarily stop the M3U gateway or Operations Agent and confirm the last valid server list remains displayed.

## Database check

```sql
SELECT status,total_channels,processed_channels,current_item,error,created_at,finished_at
FROM m3u_refresh_jobs
ORDER BY created_at DESC
LIMIT 10;
```

A failed import must preserve the previously committed active channel set, and a Jellyfin API authentication error must not invalidate the GalaxyTV panel session.
