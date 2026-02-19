# GhostWatch — User Guide

## Table of Contents

1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Permissions Setup](#permissions-setup)
4. [Getting Started](#getting-started)
5. [Menu Bar](#menu-bar)
6. [Dashboard](#dashboard)
7. [Monitors](#monitors)
8. [Settings](#settings)
9. [Export & Reports](#export--reports)
10. [Troubleshooting](#troubleshooting)
11. [Uninstallation](#uninstallation)
12. [License](#license)

---

## Introduction

GhostWatch is a macOS security monitoring application that runs silently in your menu bar. It watches 15 critical areas of your system configuration and alerts you when something changes — whether it's a new login item, a modified hosts file, a new SSH key, or a change to your firewall settings.

**Why does this matter?**

Many attacks on macOS rely on making small, silent changes to your system: adding a LaunchAgent for persistence, modifying DNS settings to redirect traffic, or installing a browser extension to steal data. These changes often go unnoticed for weeks or months. GhostWatch detects them in real time.

**Privacy first:** GhostWatch is entirely offline. No account, no telemetry, no network connections. Everything stays on your Mac.

---

## Installation

### Step 1: Download

Download the latest version from the [GitHub Releases page](../../releases). The download is a `.dmg` disk image file.

### Step 2: Install

1. Double-click the downloaded `.dmg` file to open it
2. A window will appear with GhostWatch and an Applications folder shortcut
3. **Drag GhostWatch into the Applications folder**
4. Eject the disk image (right-click > Eject in Finder)

### Step 3: First Launch

1. Open **Applications** in Finder
2. **Right-click** on GhostWatch and select **Open** (required for first launch of unsigned apps)
3. macOS will ask you to confirm — click **Open**
4. GhostWatch will appear in your menu bar (top-right of your screen) as a shield icon

> **Note:** On first launch, GhostWatch takes a baseline snapshot of your system. This is normal and takes a few seconds. All future changes will be compared against this baseline.

---

## Permissions Setup

GhostWatch needs specific macOS permissions to monitor your system effectively. Without them, some monitors will have limited coverage.

### Full Disk Access (Strongly Recommended)

**What it does:** Allows GhostWatch to read protected system databases, including the TCC privacy permissions database and all LaunchAgent/Daemon directories.

**What happens without it:** The Privacy Permissions (TCC) monitor and some LaunchAgent directories won't be accessible.

**How to grant:**

1. Open **System Settings** (Apple menu > System Settings)
2. Navigate to **Privacy & Security** > **Full Disk Access**
3. Click the lock icon at the bottom to make changes (enter your password)
4. Click the **+** button
5. Navigate to `/Applications/`, select **GhostWatch.app**, and click **Open**
6. You should see GhostWatch appear in the list with its toggle enabled
7. **Quit and relaunch GhostWatch** for the change to take effect

<p align="center">
  <img src="assets/fda-settings.png" width="500" alt="Full Disk Access settings">
</p>

### Notifications (Recommended)

**What it does:** Allows GhostWatch to display system notifications when security changes are detected.

**How to grant:**

1. Open **System Settings** > **Notifications**
2. Scroll down and find **GhostWatch**
3. Enable **Allow Notifications**
4. Set alert style to **Alerts** (recommended) — these stay on screen until you dismiss them
5. Optionally enable **Show in Notification Center** and **Badge app icon**

### Accessibility (Optional)

**What it does:** Required only if you want GhostWatch to detect certain UI-level changes.

**How to grant:**

1. Open **System Settings** > **Privacy & Security** > **Accessibility**
2. Click **+** and add GhostWatch

### Checking Permission Status

You can verify your permissions at any time:

1. Click the GhostWatch icon in the menu bar
2. Click **Settings** (or press `Cmd + ,`)
3. Go to the **Permissions** tab
4. Each permission shows a green checkmark (granted) or orange warning (not granted)
5. Click **Open Settings** next to any missing permission to go directly to the right System Settings panel

---

## Getting Started

### Understanding the Menu Bar Icon

After installation, GhostWatch lives in your menu bar. The shield icon indicates its status:

- **Normal shield:** Everything is fine, monitoring is active
- The icon itself doesn't change color — check the menu for details

### Taking a Manual Snapshot

If you want to immediately check for changes:

1. Click the GhostWatch menu bar icon
2. Click **Take Snapshot Now** (or press `Cmd + S`)
3. GhostWatch will compare the current system state against the last known state
4. Any new changes will appear as events

---

## Menu Bar

Click the GhostWatch icon in the menu bar to see:

### Quick Actions

- **Open Dashboard** (`Cmd + D`) — Opens the full dashboard window
- **Take Snapshot Now** (`Cmd + S`) — Triggers an immediate check across all monitors
- **Settings** (`Cmd + ,`) — Opens preferences
- **About GhostWatch** — Shows version and developer info

### Status

Shows whether monitoring is active and how many monitors are running (e.g., "15 monitors active").

### Recent Events

The most recent security events are shown directly in the menu for quick review. Each event shows:

- Severity icon (blue = info, orange = warning, red = critical)
- Event title
- Time of detection

### Quit

**Quit GhostWatch** (`Cmd + Q`) — Stops all monitoring and closes the app.

---

## Dashboard

The dashboard is the main interface for reviewing and managing security events. Open it from the menu bar or with `Cmd + D`.

### Sidebar (Left)

- **All Events** — Shows every detected change, with total count
- **Bookmarked** — Events you've marked for follow-up (appears only if you have bookmarks)
- **Statistics** — Charts and analytics about your security events
- **Categories** — Filter by monitor type (Login Items, Network, SSH Keys, etc.)

### Filter Bar (Top)

- **Severity toggles** — Click Info / Warning / Critical to show/hide events by severity
- **Date range picker** — Filter by: Today, 3 days, 7 days, 30 days, or All time
- **Show dismissed** (eye icon) — Toggle visibility of dismissed events
- **Export** (share icon) — Export filtered events

### Search

Use the search bar (`Cmd + F`) to find events by title, description, or app name.

### Event List (Center)

Each event card shows:

- **Severity badge** (Info / Warning / Critical)
- **Date and time** of detection
- **Title** — What changed
- **Description** — Details about the change
- **Risk assessment** — Why this matters for security (colored text)
- **Category and app name** — Which monitor detected it and which app is involved

#### Context Menu (Right-Click)

Right-click any event for quick actions:

- **Copy Title** — Copy the event title to clipboard
- **Bookmark / Remove Bookmark** — Mark for follow-up
- **Dismiss / Restore** — Hide from the main view
- **Delete** — Permanently remove the event

### Event Detail (Right)

Click any event to see its full details, including:

- Complete description and risk assessment
- Technical details
- File path (if applicable)
- Previous and new values
- Bookmark and dismiss buttons

### Statistics

The Statistics view shows:

- **7-day trend** — Bar chart of events per day
- **Category distribution** — Pie chart showing which monitors detect the most changes
- **Top applications** — Which apps trigger the most events

---

## Monitors

GhostWatch includes 15 security monitors. Each can be individually enabled or disabled in Settings.

### Core Monitors

| Monitor | What it watches | Check frequency |
|---------|----------------|-----------------|
| **Launch Services** | LaunchAgents and LaunchDaemons in system and user directories. These are programs that start automatically in the background. Malware commonly installs itself here to persist after reboots. | 30 seconds + file system events |
| **Login Items** | Applications registered to open at login. New login items could indicate unwanted software. | 30 seconds |
| **Hosts File** | The `/etc/hosts` file which maps hostnames to IP addresses. Modifications can redirect your web traffic to malicious servers or block security updates. | File system events |
| **System Security** | Secure Boot status, System Integrity Protection (SIP), and FileVault encryption. Weakened settings make your Mac vulnerable to deeper attacks. | 120 seconds |

### Extended Monitors

| Monitor | What it watches | Check frequency |
|---------|----------------|-----------------|
| **Privacy Permissions (TCC)** | The macOS privacy database that tracks which apps have access to your camera, microphone, screen recording, contacts, and more. Requires Full Disk Access. | 60 seconds |
| **Network Configuration** | DNS servers and proxy settings. Attackers may change these to intercept your internet traffic or redirect you to phishing sites. | 60 seconds |
| **Firewall** | macOS Application Firewall status and rules. The firewall controls which apps can receive incoming connections. | 120 seconds |
| **Scheduled Tasks (Cron)** | Cron jobs — commands scheduled to run at regular intervals. Malware can use these for periodic execution. | 60 seconds + file system events |
| **Configuration Profiles** | MDM and configuration profiles that can enforce system policies, install certificates, or configure VPNs. Malicious profiles can silently alter security settings. | 120 seconds |
| **Browser Extensions** | Extensions installed in Chrome, Firefox, Edge, Brave, and Arc. Malicious extensions can read passwords, track browsing, or inject content into web pages. | 120 seconds |
| **System Extensions** | Kernel extensions (kexts) and system extensions that operate at a deep level in macOS with broad system access. | 120 seconds |

### Security Monitors

| Monitor | What it watches | Check frequency |
|---------|----------------|-----------------|
| **SSH Keys** | SSH authorized keys (`~/.ssh/authorized_keys`) and public key files. An unauthorized SSH key gives an attacker passwordless remote access to your Mac. | 60 seconds + file system events |
| **User Accounts** | Local user accounts on your Mac. Unexpected account creation could indicate an attacker establishing persistent access. | 120 seconds |
| **Sharing Preferences** | Remote Login (SSH), Screen Sharing, and File Sharing services. These open your Mac to network access and should only be enabled when needed. | 120 seconds |
| **XProtect** | Apple's built-in malware protection version. Tracks updates to ensure your Mac has the latest malware definitions. | 300 seconds + file system events |

### Severity Levels

- **Info** (blue) — Normal system changes, usually benign (e.g., XProtect updated, login item removed)
- **Warning** (orange) — Potentially suspicious changes that deserve attention (e.g., new login item, new SSH key, DNS changed)
- **Critical** (red) — High-risk changes that may indicate a security issue (e.g., SIP disabled, firewall disabled, new configuration profile)

---

## Settings

Access Settings from the menu bar (`Cmd + ,`) or from the dashboard.

### General

- **Launch at login** — Start GhostWatch automatically when you log in
- **Data Retention** — How long to keep events: 30 days, 90 days, 180 days, 1 year, or forever. Old events are automatically cleaned up on launch.

### Monitors

Toggle individual monitors on or off. Each monitor has an info button (?) that explains what it watches and why.

### Permissions

View and manage the macOS permissions GhostWatch needs. Green checkmark means granted, orange warning means not granted. Click "Open Settings" to go directly to the right System Settings panel.

### Language

Switch between English and French. Requires an app restart.

### About

Shows version, build number, developer info, and license.

---

## Export & Reports

### Exporting Events

1. Open the Dashboard
2. Apply any filters you want (category, severity, date range, search)
3. Click the **Export** button (share icon) in the filter bar
4. Choose a format:
   - **JSON** — Machine-readable, good for processing with scripts
   - **CSV** — Opens in Excel, Numbers, or any spreadsheet
   - **HTML** — Styled report that opens in any web browser
5. Choose a save location
6. Click **Save**

> **Tip:** Only the currently filtered events are exported. Use this to create focused reports (e.g., "all warnings from the last 7 days").

### HTML Report

The HTML export generates a standalone report with:

- Summary cards (total events, critical/warning/info counts)
- Color-coded event table
- Severity indicators
- Full event details including risk assessments

---

## Troubleshooting

### GhostWatch doesn't appear in the menu bar

- GhostWatch is a menu bar-only app (no Dock icon by design)
- Look for the shield icon in the top-right area of your screen
- If you have many menu bar icons, it may be hidden — try expanding the menu bar area

### Some monitors show no events

- Ensure **Full Disk Access** is granted (Settings > Permissions)
- Some monitors only trigger when changes actually occur — no events means no changes were detected
- Try clicking **Take Snapshot Now** to force a check

### "Not Granted" for permissions

- Follow the permission setup steps in this guide
- After granting a permission, **quit and relaunch** GhostWatch
- Some permissions require unlocking System Settings with your password first

### Events seem delayed

- Monitors using polling (as opposed to file system events) check at intervals between 30 seconds and 5 minutes
- Use **Take Snapshot Now** for an immediate check

### App doesn't launch after macOS update

- Re-grant Full Disk Access (macOS updates sometimes reset permissions)
- Right-click the app and select Open if macOS blocks it

---

## Uninstallation

1. Click the GhostWatch menu bar icon
2. Click **Quit GhostWatch**
3. Open Finder > Applications
4. Drag GhostWatch to the Trash (or right-click > Move to Trash)
5. To remove all app data:
   ```
   rm -rf ~/Library/Application\ Support/GhostWatch
   rm -rf ~/Library/Preferences/com.ghostwatch.app.plist
   ```

---

## License

GhostWatch is freeware. Free to use for personal and commercial purposes.

You may NOT modify, reverse-engineer, decompile, or redistribute this software without prior written permission.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.

© 2026 Florian Bertaux. All rights reserved.
