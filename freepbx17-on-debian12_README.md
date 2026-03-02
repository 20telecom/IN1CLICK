# IN1CLICK — FreePBX 17 on Debian 12

# Version Update

From 1.0.1 to 1.2.0 on 2nd March 2026 21:00 GMT by kierknoby

Major update adding Heqet ISO support, live mirror monitoring via mirrors.in1.click, Debian 13 prevention, APT lock handling, and numerous hardening improvements. See changelog below.

---

## Overview

**IN1CLICK** is a streamlined, automated installer script for setting up FreePBX 17 on Debian 12. Developed by 20tele.com, the script simplifies the process by automating all necessary checks, system preparation, package installation, and post-install tasks.

---

## Disclaimer

The bash script was first published to install FreePBX 17 on 5th August 2025. Contents are provided as-is without warranty of any kind, express or implied. You may modify, distribute, and use this script; 20tele.com accepts no responsibility for any damage or issues arising from its use.

Please test it thoroughly in a controlled environment before deploying.

---

## Features

- Fast and reliable installation of FreePBX 17 on Debian 12.
- Automated pre-checks: OS version, memory, swap, architecture, hostname, disk space, existing services.
- Heqet ISO detection with unattended install support and automatic cleanup on completion.
- Live mirror monitoring via mirrors.in1.click with 3 consecutive stability checks before proceeding.
- Interactive retry menu when mirrors are unstable (Heqet auto-abandons with guidance).
- Saturday mirror warning for known busy periods on the official FreePBX mirrors.
- Pre-upgrade mirror re-check before module upgrades, with graceful skip if mirrors have degraded.
- Debian 13 prevention: blocks stable and trixie references in APT sources before and after updates.
- Disables unattended-upgrades to prevent APT lock conflicts during install.
- APT lock wait checks before package updates and before FreePBX installation (5 min / 2 min timeouts).
- Validates IP assignment (static or DHCP).
- Verifies and installs required packages including curl, iptables, and others.
- Confirms availability of deb.freepbx.org and the FreePBX GitHub installer before continuing.
- Checks outbound internet connectivity and displays the public IP.
- OS version re-check to ensure Debian did not upgrade from 12 (bookworm) to 13 (trixie).
- Detects desktop environments and warns users to use a minimal server install.
- Auto-fixes numeric-only hostnames to freepbx.sangoma.local.
- Handles missing /etc/apt/sources.list (newer Debian .sources format).
- Uses the official FreePBX install script from Sangoma.
- Automatically upgrades modules and reloads FreePBX (skips gracefully if mirrors are bad).
- Post-install verification that Apache is running and port 80 is accessible.
- Cleans up Asterisk logs and system mail.
- Clears bash history on completion.
- Removes itself from disk after installation (manual installs only, skipped on Heqet).
- Heqet-specific error messages when FreePBX, Asterisk, or MariaDB are already installed (guides user to boot from ISO again).
- Getty restore on tty1 for Heqet ISO installs.

---

## Requirements

- Debian 12 (bookworm), 64-bit (x86_64)
- Root access
- Internet connectivity
- No existing installation of FreePBX, Asterisk, or MariaDB
- Minimal server install (no desktop environment)

---

## One-Click Installation on Debian 12

Option 1: On a fresh Debian 12 installation, run this curl command as root:

```bash
curl freepbx.in1.click | sh
```

Option 2: On a fresh Debian 12 installation, run this wget command as root:

```bash
wget https://raw.githubusercontent.com/20telecom/IN1CLICK/main/freepbx17-on-debian12 -O /tmp/IN1CLICK && chmod +x /tmp/IN1CLICK && /tmp/IN1CLICK
```

---

## Sample Output

```
Hello. Thanks for trying IN1CLICK for FreePBX 17 on Debian 12 (bookworm).

In case you need support from 20tele.com, this is IN1CLICK version 1.2.0.

Disabling unattended-upgrades for this session...
Unattended upgrades stopped. OK to proceed.

Checking if this is the Heqet ISO...
Not the Heqet ISO. OK to proceed.

Checking if this is Debian 12...
Debian 12 confirmed. OK to proceed.

Checking available disk space...
Disk space available: 28.50 GB. OK to proceed.

Checking available memory and swap...
Memory: 2048 MB, Swap: 512 MB. OK to proceed.

Checking system architecture...
Architecture is 64-bit. OK to proceed.

Checking hostname label format...
Hostname label appears valid. OK to proceed.

Checking for desktop environment...
No desktop environment detected. OK to proceed.

Checking /tmp permissions...
/tmp is writable and executable. OK to proceed.

Checking for existing FreePBX installation...
No existing FreePBX found. OK to proceed.

Checking for existing Asterisk installation...
No existing Asterisk found. OK to proceed.

Checking for existing MariaDB installation...
No existing MariaDB found. OK to proceed.

Checking for existing Node.js installation...
No existing Node.js installation found. OK to proceed.

Checking APT sources and update availability...
APT sources appear to be valid. OK to proceed.

Checking and fixing forbidden APT sources (stable/trixie)...
APT sources do not contain forbidden entries. OK to proceed.

Checking for APT locks before updating packages...
No APT locks detected. OK to proceed.

Updating package lists...
All package lists updated. OK to proceed.

Verifying and fixing no Debian 13 (trixie) references after update...
No 'trixie' references found after update. OK to proceed.

Upgrading packages... Please be patient.
Packages upgraded successfully. OK to proceed.

Checking this is still Debian 12...
Debian 12 confirmed. OK to proceed.

Checking IP assignment type...
Static IP detected. OK to proceed.

Checking for iptables...
iptables is already installed. OK to proceed.

Checking for active iptables rules...
No active DROP/REJECT iptables rules detected. OK to proceed.

Checking for port 80 conflicts...
No port 80 conflicts detected. OK to proceed.

Checking DNS resolution...
DNS resolution working. OK to proceed.

Checking for curl...
curl is installed. OK to proceed.

Using mirrors.in1.click to check the official FreePBX mirrors... [attempt 1]

[...mirror checker output...]

Mirrors stable. It should be safe to proceed with module updates.
Check out https://in1.click/mirrors in a web browser. (1/3 passed)
  Retrying in 30...

Using mirrors.in1.click to check the official FreePBX mirrors... [attempt 2]

[...mirror checker output...]

Mirrors stable. It should be safe to proceed with module updates.
Check out https://in1.click/mirrors in a web browser. (2/3 passed)
  Retrying in 30...

Using mirrors.in1.click to check the official FreePBX mirrors... [attempt 3]

[...mirror checker output...]

Mirrors stable. It should be safe to proceed with module updates.
Check out https://in1.click/mirrors in a web browser. (3/3 passed)

All 3 checks passed. Great job, Sangoma! OK to proceed.

Checking for FreePBX GitHub installer at raw.githubusercontent.com...
FreePBX GitHub installer is reachable. OK to proceed.

Checking FreePBX installation repository (deb.freepbx.org)...
FreePBX APT repository is reachable. OK to proceed.

Checking outbound internet connectivity (public IP)...
Outbound internet connectivity confirmed. Your Public IP is 203.0.113.45. OK to proceed.

Checking for APT locks before installing FreePBX 17...
No APT locks detected. OK to proceed.

Installing FreePBX 17...
[...Sangoma installer output...]

Using mirrors.in1.click to check the official FreePBX mirrors... [attempt 1/3]

[...mirror checker output...]

Check out https://in1.click/mirrors in a web browser. Upgrading modules...

[...fwconsole ma upgradeall output...]

Modules upgraded and system reloaded. OK to proceed.

Checking if Apache is running...
Apache is running. OK to proceed.

Checking access to the FreePBX GUI...
Port 80 is open on 203.0.113.45. OK to proceed.

Cleaning Asterisk logs...
Full, fail2ban, and root mail cleared. OK to proceed.

Clearing bash history...
Bash history cleared. OK to proceed.

Attempting to delete IN1CLICK by 20tele.com: /tmp/IN1CLICK
IN1CLICK by 20tele.com removed successfully.

IN1CLICK completed in 15 min 20 sec.

Goodbye.
```

---

## Changelog

### 1.2.0 (2nd March 2026)

**New: Heqet ISO support**
- Detect Heqet via firstboot/cleanup service files, set IS_HEQET flag.
- Heqet-specific errors for FreePBX/Asterisk/MariaDB already installed (boot ISO again, contact support).
- Heqet cleanup block: mark install complete, schedule service removal via cron, remove preseed files.
- Getty restore on tty1 (Heqet only).

**New: Mirror monitoring via mirrors.in1.click**
- Replace individual curl checks (mirror.freepbx.org, ftp.debian.org) with mirrors.in1.click integration.
- 3 consecutive checks required, 30 second countdown between each.
- Status-specific feedback: stable, degraded, down, server struggling, unreachable.
- Heqet: auto-abandon after 3 failures with guidance (monitor mirrors, boot ISO, Saturday warning).
- Manual: interactive menu (retry / abort / carry on anyway).
- Pre-upgrade mirror re-check before fwconsole ma upgradeall, skip gracefully if bad.

**New: Debian 13 prevention**
- Block stable and trixie in APT sources before and after apt update.
- Replace stable with bookworm, comment out trixie lines.

**New: APT lock handling**
- Stop unattended-upgrades, apt-daily, apt-daily-upgrade before any APT operations.
- Two lock wait checks (before apt update, before FreePBX install) with 5 min / 2 min timeouts.

**New: Hostname auto-fix**
- Numeric-only hostnames auto-corrected to freepbx.sangoma.local.

**New: Outbound connectivity check**
- Public IP check via ifconfig.me.

**New: Port 80 verification**
- Post-install check that FreePBX GUI is reachable via nc.

**Changed**
- Welcome message restyled: cyan greeting, yellow version line, removed version from greeting.
- APT source check handles missing /etc/apt/sources.list (newer Debian .sources format).
- /tmp noexec check fixed (was grep -q piped to grep -q, never matched).
- deb.freepbx.org check kept as separate gate (different infrastructure to module mirrors).

**Removed**
- Reboot countdown and auto-reboot.
- Legacy mirror check (mirror.freepbx.org).
- Debian base mirror check (ftp.debian.org).

### 1.1.0 (11th February 2026)

- Further improvements to mirror logic and user prompts.
- Documentation and output clarified.

### 1.0.1 (15th August 2025)

- Added "Operating System Check 2" to ensure system is still Debian 12 (bookworm) after update/upgrade and before proceeding.

### 1.0.0 (5th August 2025)

- Initial release.

---

## Support

If something goes wrong during install, check the logs:

- `/var/log/pbx/freepbx-*.log`
- `/var/log/asterisk/full`
- `systemctl status asterisk`
- `fwconsole restart`

If you need help, email support@20tele.com or open a ticket at https://support.20tele.com

---

## License

GNU General Public License v3.0

---
