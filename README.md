<img width="2154" height="749" alt="Screenshot from 2026-07-16 15-15-15" src="https://github.com/user-attachments/assets/05a30ba1-2b6e-4cb8-8533-0b128ecf8920" />
<img width="2154" height="749" alt="Screenshot from 2026-07-16 15-17-01" src="https://github.com/user-attachments/assets/5a8f57b4-d945-4367-99c1-4eaa8e05ec6c" />
# JellyPanel
Jellyfin Panel with more features
GalaxyTV Jellyfin Management Suite v1.0

A complete self-hosted media-server management platform built around Jellyfin.

GalaxyTV turns a standard Jellyfin installation into a managed service with user accounts, subscription packages, resellers, Live TV, EPG automation, cloud-media libraries, stream monitoring, backups, and server operations—all controlled from one clean web panel.

Main Features
Easy Server Installation
One ZIP package.
Guided installation on Ubuntu 24.04.
Automatic Docker and Docker Compose setup.
Fresh install, upgrade, repair, restore, diagnostics, and uninstall modes.
Configurable panel, Jellyfin, and internal service ports.
Configurable media paths, timezone, and rclone settings.
Automatic pre-upgrade backup and rollback protection.
Designed to coexist with Emby, Plex, and other services using custom ports.
Jellyfin User Management
Create, edit, enable, disable, and remove Jellyfin users.
Account expiration dates.
Automatic account suspension when expired.
Re-enable renewed accounts.
Disable downloads for customer accounts.
Prevent customers from changing restricted account settings.
Assign subscription packages to users.
Configure connection limits by package.
Keep administrative accounts protected from reseller actions.
Subscription Packages

Create custom plans such as:

1 Connection – 30 Days
2 Connections – 90 Days
4 Connections – 1 Year
Movies Only
Live TV Only
Premium Full Access

Each package can control:

Expiration length.
Simultaneous connection limit.
Live TV access.
Movie access.
TV-show access.
Download permissions.
Package price or reseller credit cost.
Reseller System
Administrator, master-reseller, and reseller roles.
Reseller credit balances.
Credit transaction history.
Resellers can create and renew customers.
Resellers only see their own customers.
Master resellers can manage child resellers.
Package-based credit deductions.
Protected administrator settings.
Full reseller activity records.
Live TV and M3U Management
Add multiple M3U playlists.
Enable or disable individual sources.
Filter channels by category.
Build customer-specific filtered playlists.
Preserve channel names, logos, groups, and tvg-id information.
Detect EPG URLs embedded in M3U headers.
Manually assign one or more XMLTV feeds.
Test playlists and guide feeds before applying them.
Refresh Jellyfin Live TV directly from GalaxyTV.
Automatic EPG Mapper
Exact tvg-id matching.
Channel-name matching.
Channel-number matching.
Normalized matching that removes unnecessary HD, FHD, country, and punctuation tags.
Confidence scoring.
Ambiguous-match protection.
Manual channel-to-EPG mapping.
Permanent saved overrides.
Duplicate XMLTV ID detection.
Multiple EPG feeds with priority order.
Preview all mappings before applying changes.
Repair missing tvg-id values without modifying the original provider playlist.
Movies and TV-Show Library Mirror

GalaxyTV can build a lightweight local Jellyfin library from media stored on rclone, Google Drive, network storage, or other mounted storage.

Walks both movie and TV-show folders.
Supports standard movie title and year naming.
Detects common TV naming formats such as S01E01 and 1x01.
TMDB movie, show, season, and episode metadata.
Generates local STRM files.
Generates Jellyfin-compatible NFO metadata.
Downloads posters and backdrop images.
Preserves TMDB and IMDb IDs when available.
Incremental updates process only changed files.
Missing files can be removed automatically.
Local metadata scanning reduces unnecessary cloud-drive access.
Stable internal playback links continue working even when library names change.
Stream Operations

See what customers are watching in real time:

Username and reseller.
Remote IP address.
Device and application.
Movie, episode, or Live TV channel.
Playback progress.
Paused or playing status.
Direct Play, Direct Stream, or Transcoding.
Video and audio codecs.
Resolution and bitrate.
Current connection usage.
Package connection allowance.

Available administrator controls:

Send an on-screen message.
Stop an individual playback session.
Suspend a user.
Re-enable a suspended user.
Review and resolve warnings.
Optionally stop excess sessions automatically.
Abuse and Connection Monitoring
Detect users exceeding package limits.
Detect several IP addresses using one account.
Detect repeated session reconnects.
Record session history.
Separate warnings from automatic enforcement.
Configurable monitoring thresholds.
Resellers only see warnings belonging to their customers.
Automatic enforcement is optional and disabled by default.
Operations Center

Monitor the entire installation from one page:

Jellyfin service health.
PostgreSQL health.
Redis health.
GalaxyTV API and worker health.
M3U and media-gateway health.
Rclone mount responsiveness.
Media-drive and system-drive free space.
Last media inventory scan.
Last library-mirror build.
M3U and XMLTV errors.
Active Jellyfin sessions.
Failed metadata items.
Controlled service restart options.
Backup and Recovery
Manual backups.
Automatic nightly backups.
Daily and weekly retention policies.
PostgreSQL database backup.
Jellyfin configuration and database backup.
GalaxyTV users, packages, resellers, credits, and settings.
M3U, XMLTV, and EPG mappings.
Optional STRM and NFO mirror backup.
Checksum verification before restoration.
Restore from the GalaxyTV panel.
Automatic backup before upgrades.
Installation rollback when validation fails.
Diagnostics and Support
One-click diagnostic ZIP.
Sensitive passwords and API keys are automatically redacted.
Container status and health information.
Recent service logs.
Mount and disk information.
Database migration information.
M3U, EPG, mirror, and session status.
Simple support package users can send for troubleshooting.
Designed For

GalaxyTV is suitable for:

Personal media-server owners.
Private organizations.
Hotels and hospitality systems.
Community and educational media libraries.
Managed Jellyfin hosting.
Reseller-based customer management.
Operators distributing media they own or are authorized to distribute.

GalaxyTV Jellyfin Management Suite v1.0 is now available.

Turn a regular Jellyfin server into a complete managed media platform with:

✅ User expiration and renewals
✅ Subscription packages and connection limits
✅ Reseller and credit management
✅ Multiple M3U and XMLTV sources
✅ Automatic EPG mapping
✅ Movies and TV-show STRM libraries
✅ TMDB metadata, NFO files, posters, and backdrops
✅ Real-time playback monitoring
✅ Session messages and playback controls
✅ Account-abuse warnings
✅ Automatic backups and restore
✅ Server health and rclone monitoring
✅ One-click redacted diagnostics
✅ Guided Ubuntu 24.04 installation
✅ Fresh install, upgrade, repair, and rollback modes

One ZIP. One installer. One management panel.

No media or television services are included. You supply your own authorized content and sources.
