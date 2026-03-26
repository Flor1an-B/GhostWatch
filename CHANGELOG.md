# Changelog

All notable changes to GhostWatch are documented in this file.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [3.2.1] — 2026-03-26

### Fixed

- **Login Items — UUID display:** Login items registered with instance-specific UUID suffixes
  (e.g., Ollama, Electron-based apps) now resolve to their correct application name instead of
  displaying a raw UUID like `B71F6815-9AF4-428C-AF31-CCCE43F3B611`. The bundle ID extraction
  logic now strips both numeric PID components and UUID-format identifiers from launchctl labels
  before querying `NSWorkspace`.

- **Dashboard sidebar — date filter not applied to counts:** The category sidebar (Login Items,
  Privacy Permissions, Browser Extensions, etc.) and the severity breakdown bar (Info / Warning /
  Critical / total count) now correctly reflect the selected time range (Today, 3 days, 7 days,
  30 days). Previously these components always displayed all-time totals regardless of the active
  filter.

- **Permissions — Full Disk Access status always showing "Not Granted":** The FDA detection now
  works correctly. The Settings panel re-checks all permission statuses when the app returns to
  the foreground, so granting permissions in System Settings is immediately reflected.

- **Notifications — not delivered after permission granted:** The notification manager now queries
  the live authorization status from `UNUserNotificationCenter` before each delivery instead of
  relying on a flag set once at launch. Granting or revoking notification permission in System
  Settings takes effect immediately without restarting the app.

### Security

- **App restart mechanism hardened:** The language-change restart no longer uses a shell command
  (`/bin/sh -c`) with string interpolation. It now calls `/usr/bin/open` directly via the Process
  API with array arguments, eliminating any command injection surface.

- **FileWatcher symlink protection:** The file watcher now verifies that monitored paths are not
  symbolic links before opening them. This prevents an attacker with local access from redirecting
  monitoring to an arbitrary directory via symlink substitution.

- **Database migration logging:** When the SwiftData store requires a reset after a failed
  migration, the operation is now logged with full detail (individual file deletions and any
  errors) instead of being silently suppressed with `try?`.

### Changed

- **Removed App Sandbox:** GhostWatch is no longer sandboxed. The sandbox was incompatible with
  the app's purpose as a system-wide security monitor — temporary-exception entitlements were not
  honoured for ad-hoc signed builds, preventing reliable access to protected system files (TCC
  database, LaunchDaemons, etc.). Without the sandbox, Full Disk Access works as expected and all
  monitors can access the paths they need directly.

- **Proper ad-hoc code signing:** The app is now correctly signed with an ad-hoc signature
  including Hardened Runtime and sealed resources. This ensures macOS TCC can identify the app and
  enforce granted permissions (FDA, Notifications, Accessibility).

> **Note:** The first launch after updating from 3.2.0 starts with a fresh event database.
> The SwiftData store location changed from the sandbox container to
> `~/Library/Application Support/`.

---

## [3.2.0] — 2026-03-01

### Added

- **Phase 2 — Extended Monitoring (5 new monitors):**
  - Privacy Permissions (TCC database)
  - Network Configuration (DNS & proxies)
  - Firewall status and rules
  - Scheduled Tasks (Cron jobs)
  - Configuration Profiles (MDM)
- Dashboard severity breakdown bar
- Settings panel with all 9 monitor toggles
- `SQLiteReader` utility for safe, read-only TCC.db access (parameterized queries)

### Changed

- Minimum macOS requirement bumped to **14.0 (Sonoma)** to support SwiftData

---

## [3.1.0] — 2026-02-15

### Added

- Statistics view: 7-day trend chart, category distribution, top applications
- Export to JSON, CSV, and HTML
- Bookmark and dismiss events
- Full-text search across events
- French localisation

---

## [3.0.0] — 2026-02-01

### Added

- Phase 1 MVP
- App shell with `MenuBarExtra` (LSUIElement)
- 4 core monitors: Launch Services, Login Items, Hosts File, System Security
- Alert engine with severity classification and deduplication
- SwiftData persistence (`SystemEvent` model)
- Event timeline UI with category sidebar and detail panel
- macOS native notifications
- Settings with data retention policy
