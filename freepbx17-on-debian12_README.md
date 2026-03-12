# IN1CLICK: FreePBX 17 on Debian 12

# Version Update

From 1.3.0 to 1.3.1 on 12th March 2026 by kierknoby

Cosmetic changes. 

From 1.2.1 to 1.3.0 on 11th March 2026 by kierknoby

Added screen session support for interactive SSH installs, non-interactive (cloud-init/pipe) detection, reboot-required handling after package upgrades, an improved memory check with interactive abort option, APT source auto-rewrite for non-official mirrors, provider mirror list removal, a Ctrl+C trap during FreePBX installation, a stray file scan on self-removal (common paths first, then full filesystem), and a full Apache configuration and GUI verification step with ionCube detection and retry logic. Removed the post-install mirror re-check. See changelog below.

---

## Overview

**IN1CLICK** is a streamlined, automated installer script for setting up FreePBX 17 on Debian 12. Developed by 20tele.com, the script simplifies the process by automating all necessary checks, system preparation, package installation, and post-install tasks.

---

## What You Get

- Debian 12 (Bookworm) - Latest stable release
- FreePBX 17 - Latest version
- Asterisk 22 - Telephony engine
- MariaDB - Database server
- Apache2 + PHP - Web server and runtime
- All dependencies - Fully configured

---

## Requirements

- Debian 12 (bookworm), 64-bit (x86_64)
- Root access
- Internet connectivity
- No existing installation of FreePBX, Asterisk, or MariaDB
- Minimal server install (no desktop environment)

---

## One-Click Installation on Debian 12

Option 1: run this curl command as root on a fresh Debian 12 installation:

```bash
curl freepbx.in1.click | sh
```

Option 2: run this wget command as root on a fresh Debian 12 installation:

```bash
wget https://raw.githubusercontent.com/20telecom/IN1CLICK/main/freepbx17-on-debian12 -O /tmp/IN1CLICK && chmod +x /tmp/IN1CLICK && /tmp/IN1CLICK
```

Option 3: cloud-init User Data on first boot of a fresh Debian 12 installation:
```bash
#cloud-config
runcmd:
  - wget https://raw.githubusercontent.com/20telecom/IN1CLICK/main/freepbx17-on-debian12 -O /tmp/IN1CLICK
  - chmod +x /tmp/IN1CLICK
  - /tmp/IN1CLICK
```
---

## Features

- Fast and reliable installation of FreePBX 17 on Debian 12.
- Automated pre-checks: OS version, memory, swap, architecture, hostname, disk space, existing services.
- Heqet ISO detection with unattended install support and automatic cleanup on completion.
- Non-interactive detection: cloud-init, pipe, and no-TTY environments skip screen re-launch and run directly.
- Screen session support: interactive SSH installs are relaunched inside screen to survive disconnection.
- Reconnection hook: dropped into /etc/profile.d/ so a reconnecting user is prompted to reattach, leave it running, or abort (with elapsed time warning). Removed on successful completion.
- Live mirror monitoring via mirrors.in1.click with 3 consecutive stability checks before proceeding.
- Integrated deb.freepbx.org (APT repo) health check within the mirror gate.
- Interactive retry menu when mirrors are unstable (Heqet and non-interactive auto-abandon with guidance).
- Saturday mirror warning for known busy periods on the official FreePBX mirrors.
- Debian 13 prevention: blocks stable and trixie references in APT sources before and after updates.
- Disables unattended-upgrades to prevent APT lock conflicts during install.
- APT lock wait checks before package updates and before FreePBX installation (5 min / 2 min timeouts).
- Reboot-required detection: if a kernel or library update requires a reboot, the script prompts the user and exits cleanly (skipped on Heqet and non-interactive).
- Validates IP assignment (static or DHCP).
- Verifies and installs required packages including curl, iptables, and others.
- Checks outbound internet connectivity and displays the public IP.
- OS version re-check to ensure Debian did not upgrade from 12 (bookworm) to 13 (trixie).
- Detects desktop environments and warns users to use a minimal server install.
- Auto-fixes numeric-only hostnames to freepbx.sangoma.local.
- APT source auto-rewrite: rewrites sources.list to deb.debian.org if no official Debian mirrors are found. Removes provider mirror list files (e.g. DigitalOcean).
- Handles missing /etc/apt/sources.list (newer Debian .sources format).
- Uses the official FreePBX install script from Sangoma.
- Ctrl+C trap during FreePBX installation to show a clear failure message rather than exiting silently.
- Automatically upgrades modules and reloads FreePBX after installation.
- Post-install Apache configuration: enables required modules, activates FreePBX site config, adds root redirect to /admin/, and verifies the setup page loads correctly with up to 3 retries.
- Cleans up Asterisk logs and system mail.
- Clears bash history on completion.
- Removes itself from disk after installation (manual installs only, skipped on Heqet). Checks common paths first, then performs a full filesystem scan to confirm no stray copies remain.
- Heqet-specific error messages when FreePBX, Asterisk, or MariaDB are already installed (guides user to boot from ISO again).
- Getty restore on tty1 for Heqet ISO installs.

---

## Security & Upgrade Safety

**APT Source Auto-Fix:**
- 'stable' is replaced with 'bookworm' automatically.
- 'trixie' lines are commented out.
- All sources are checked again after `apt update`.
- If no official Debian mirrors are found, sources.list is rewritten to deb.debian.org.
- Provider mirror list files (e.g. DigitalOcean) are removed automatically.
- Warnings are printed if changes are made.

**Upgrade Blocking:**
- IN1CLICK will not allow upgrades to Debian 13 (Trixie).
- Only Bookworm sources are supported.

**APT Lock Protection:**
- Unattended-upgrades, apt-daily, and apt-daily-upgrade are stopped before any APT operations.
- Lock wait checks run before package updates (5 min timeout) and before FreePBX installation (2 min timeout).

**Reboot Handling:**
- If a reboot is required after the package upgrade, the script detects /var/run/reboot-required and prompts the user to reboot before continuing. Skipped on Heqet and non-interactive installs.

**Mirror Gate:**
- 3 consecutive successful checks via mirrors.in1.click are required before proceeding.
- Both module mirror status and deb.freepbx.org status must pass.
- If mirrors are unstable, manual installs get an interactive retry menu; Heqet and non-interactive installs auto-abandon with guidance.

**Screen and Disconnection Safety:**
- Interactive SSH installs run inside a screen session. If the SSH connection drops, the install continues in the background.
- A reconnection hook in /etc/profile.d/ prompts the user on re-login to reattach, leave it running, or abort.

**Self-Cleanup:**
- IN1CLICK removes itself from disk after a successful manual install. It checks common paths first, then performs a full filesystem scan for stray copies.
- Bash history is cleared on completion.
- Asterisk logs and system mail are cleaned.

---

## Sample Output

```
Hello. Thanks for trying IN1CLICK for FreePBX 17 on Debian 12 (bookworm).

In case you need support from 20tele.com, this is IN1CLICK version 1.3.1.

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

Checking APT sources and fixing if necessary...
APT sources already point to official Debian mirrors. OK to proceed.

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

Using mirrors.in1.click to check the official FreePBX mirrors... (attempt 1/3)

[...mirror checker output...]

Mirrors stable. It should be safe to proceed with module updates.
Check out https://in1.click/mirrors in a web browser. (1/3 passed)
  Retrying in 30...

Using mirrors.in1.click to check the official FreePBX mirrors... (attempt 2/3)

[...mirror checker output...]

Mirrors stable. It should be safe to proceed with module updates.
Check out https://in1.click/mirrors in a web browser. (2/3 passed)
  Retrying in 30...

Using mirrors.in1.click to check the official FreePBX mirrors... (attempt 3/3)

[...mirror checker output...]

Mirrors stable. It should be safe to proceed with module updates.
Check out https://in1.click/mirrors in a web browser. (3/3 passed)

All 3 checks passed. Great job, Sangoma! OK to proceed.

Checking for FreePBX GitHub installer at raw.githubusercontent.com...
FreePBX GitHub installer is reachable. OK to proceed.

Checking outbound internet connectivity (public IP)...
Outbound internet connectivity confirmed. Your Public IP is 123.45.67.89. OK to proceed.

Checking for APT locks before installing FreePBX 17...
No APT locks detected. OK to proceed.

Pre-flight checks complete. Preparing for takeoff, fasten your seatbelts.

Installing FreePBX 17...
[...Sangoma installer output...]

Upgrading FreePBX modules...
[...fwconsole ma upgradeall output...]

Setting correct file ownership...
File ownership set. OK to proceed.

Reloading FreePBX...
Modules upgraded and system reloaded. OK to proceed.

Checking if Apache is running...
Apache is running. OK to proceed.

Configuring Apache and verifying FreePBX GUI...
Root redirect to /admin/ added. OK to proceed.
FreePBX setup page confirmed at http://123.45.67.89. OK to proceed.

Cleaning Asterisk logs...
Full, fail2ban, and root mail cleared. OK to proceed.

IN1CLICK first-boot detected. Cleaning up...

  - Install marked as complete

  - in1click-firstboot.service scheduled for removal on next boot

  - in1click-cleanup.service scheduled for removal on next boot

  - Preseed files cleaned up

Cleanup complete. OK to proceed.

Clearing bash history...

Removing all traces of IN1CLICK...
Attempting to delete IN1CLICK by 20tele.com: /tmp/IN1CLICK
The temporary IN1CLICK script was removed successfully.
Checking... IN1CLICK removal confirmed. OK to proceed.

Thanks for trying IN1CLICK 1.3.1. FreePBX 17 is now ready to use.

Please go to http://123.45.67.89 in your preferred web browser.

IN1CLICK completed in 14 min 59 sec.

Goodbye.
```

---

## Changelog

### 1.3.1 (11th March 2026)

**Cosmetic changes**
- Minor echo changes.

### 1.3.0 (11th March 2026)

**New: Apache configuration and GUI verification**
- Simple nc port 80 check replaced with a full apache_configure function that enables rewrite/expires/headers modules, activates freepbx.conf, and injects a root redirect to /admin/ if not already present.
- After configuration, curls /admin/config.php and checks for the FreePBX setup page, with up to 3 retries.
- Detects ionCube Loader errors and calls handle_install_failure immediately.
- Detects the Apache default page and re-applies config before retrying.
- On failure after 3 attempts, Heqet and non-interactive installs exit; manual installs pause and wait for Enter.

**New: Screen session support**
- Interactive SSH installs are relaunched inside a screen session using --skip-checks so the install survives disconnection.
- Non-interactive environments (cloud-init, pipe, no TTY) are detected via IS_NONINTERACTIVE and skip screen entirely.
- A reconnection hook is written to /etc/profile.d/in1click-reattach.sh before screen launches. On re-login, the user is prompted to reattach, leave it running, or abort. The elapsed time is shown as a warning before abort is confirmed. The file is removed on successful completion.

**New: Reboot-required handling**
- After the package upgrade, /var/run/reboot-required is checked. If present, the script prints the recommended curl command, prompts for Enter, and reboots. Skipped on Heqet and non-interactive installs.

**New: Ctrl+C trap during install**
- A trap on INT is set for the FreePBX installation step, calling handle_install_failure on interruption rather than exiting silently.

**New: Improved memory check**
- Now distinguishes critically low RAM (under 900 MB with no swap) from nominally low RAM (under 1000 MB). Critically low triggers an interactive abort/continue prompt on manual installs, or auto-continues on Heqet and non-interactive installs with swap instructions printed.

**New: APT source auto-rewrite**
- If no official Debian mirrors are detected, sources.list is rewritten to deb.debian.org and security.debian.org rather than just warning and continuing.
- Provider mirror list files in /etc/apt/mirrors/ are removed.
- Provider-managed debian.sources files (mirrorlist/mirror+file references) are removed.
- DigitalOcean and other provider entries in sources.list.d/ are removed.

**New: Stray file scan on self-removal**
- After deleting the script, common paths (/tmp, /usr/local/bin, /root) are checked first, then the full filesystem is scanned for any remaining IN1CLICK copies and a warning is printed if any are found.

**Removed: Post-install mirror re-check**
- The 3-attempt mirror re-check loop before fwconsole ma upgradeall has been removed. Modules are now upgraded unconditionally after install. The pre-install mirror gate is considered sufficient.

**Changed: fwconsole chown and fwconsole reload promoted to named steps**
- Each now has its own print_step header and confirmation message.

**Changed: IS_NONINTERACTIVE included in mirror auto-abandon condition**
- Previously only IS_HEQET triggered auto-abandon when mirrors failed. Non-interactive installs now also auto-abandon rather than blocking on the interactive retry menu.

**Changed: IS_HEQET initialised before SKIP_CHECKS block**
- IS_HEQET=false is now set at the top of the script, outside both the pre-flight and install blocks, so the install block can reference it correctly when re-launched inside screen with --skip-checks.

**Changed: package upgrade uses NEEDRESTART_MODE=a**
- Suppresses needrestart interactive prompts during the upgrade step.

**Cosmetic changes**
- Sample output updated to reflect 1.3.0.
- Mirror attempt format changed from [attempt N] to (attempt N/3).
- APT source step header updated to reflect the new auto-rewrite behaviour.
- Install block opens with a "Pre-flight checks complete" banner and version reminder.
- Mirror unreachable message updated to mention possible Cloudflare blocking.
- Heqet mirror abandon guidance updated to say "run the installer again" rather than "boot from the ISO".
- Minor echo changes.

### 1.2.1 (3rd March 2026)

**Changed: deb.freepbx.org check merged into mirror gate**
- Removed the standalone "Checking FreePBX installation repository (deb.freepbx.org)..." block.
- Added deb_status variable and parsing in mirror_check_once to grep cli.sh output for Packages.gz valid, captcha, installations will fail, or not valid gzip data.
- Gate logic now requires both mirror_status and deb_status to be good for a pass.
- New feedback when mirrors pass but deb fails: "Module mirrors are stable but deb.freepbx.org (APT repo) is not healthy."

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

**Removed**
- Reboot countdown and auto-reboot.
- Legacy mirror check (mirror.freepbx.org).
- Debian base mirror check (ftp.debian.org).
- Standalone deb.freepbx.org check (now part of mirror gate).

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

## Credits

- IN1CLICK Script: Developed by 20tele.com
- FreePBX: Sangoma Technologies
- Debian: Debian Project
- Mirror Monitoring: mirrors.in1.click by 20tele.com

---

## Contributing

Contributions are welcome! Please:
1. Test thoroughly in a controlled environment
2. Follow code style
3. Document changes
4. Submit pull requests with details

---

## Disclaimer

This software is provided as-is without warranty of any kind, express or implied. You may modify, distribute, and use this script; 20tele.com accepts no responsibility for any damage or issues arising from its use. Please test thoroughly in a controlled environment before deploying.

---

## License

GNU General Public License v3.0

---
