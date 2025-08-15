# IN1CLICK — FreePBX 17 on Debian 12 [1.0.1]

# Version Update

From 1.0.0 to 1.0.1 on 15th August 2025 16:15 BST by kierknoby

Added "Operating System Check 2" to ensure system is still Debian 12 (bookworm) after update/upgrade and before proceeding. This is now freepbx17-on-debian12 [1.0.1].

---

## Overview

**IN1CLICK** is a streamlined, automated installer script for setting up FreePBX 17 on Debian 12. Developed by 20tele.com, the script simplifies the process by automating all necessary checks, system preparation, package installation, and post-install tasks. It also removes itself on completion, leaving no trace behind.

---

## Disclaimer

The bash script was first published to install FreePBX 17 on 5th August 2025. Contents are provided as-is without warranty of any kind, express or implied. You may modify, distribute, and use this script; 20tele.com accepts no responsibility for any damage or issues arising from its use.

Please test it thoroughly in a controlled environment before deploying.

---

## Features

- Fast and reliable installation of FreePBX 17 on Debian 12.
- Automated pre-checks: OS version, memory, swap, architecture, hostname, disk space, existing services.
- Validates IP assignment (static or DHCP).
- Verifies and installs required packages including curl, iptables, and others.
- Confirms availability of Debian and FreePBX mirrors before continuing.
- OS version re-check to ensure Debian did not upgrade from 12 (bookworm) to 13 (trixie).
- Detects desktop environments and warns users to use a minimal server install.
- Uses the official FreePBX install script from Sangoma.
- Automatically upgrades modules and reloads FreePBX.
- Cleans up Asterisk logs and system mail.
- Clears bash history and removes itself from disk after installation.
- Reboots system automatically after notifying the user.

---

## Requirements

- Debian 12 (bookworm), 64-bit (x86_64)
- Root access
- Internet connectivity
- No existing installation of FreePBX, Asterisk, or MariaDB
- Minimal server install (no desktop environment)

---

## One-Click Installation

Run this command on a clean Debian 12 system as root:

```bash
wget https://raw.githubusercontent.com/20telecom/IN1CLICK/main/freepbx17-on-debian12 -O /tmp/IN1CLICK && chmod +x /tmp/IN1CLICK && /tmp/IN1CLICK
```

---

## Sample Output

```
Hello. Thanks for using IN1CLICK for FreePBX 17 on Debian 12 [1.0.0] by 20tele.com

Checking if this is Debian 12...
Debian 12 confirmed. OK to proceed.

Checking available disk space...
Disk space available: 16.47 GB. OK to proceed.

Checking available memory and swap...
Memory: 954 MB, Swap: 2399 MB. OK to proceed.

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

Updating package lists...
All package lists updated. OK to proceed.

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

Checking for FreePBX GitHub installer at raw.githubusercontent.com...
FreePBX GitHub installer is reachable. OK to proceed.

Checking FreePBX mirrors...
FreePBX APT mirror is reachable. OK to proceed.

Legacy FreePBX mirror is reachable. OK to proceed.

Debian base mirror is reachable. OK to proceed.

Installing FreePBX 17...
Modules upgraded and system reloaded. OK to proceed.

Checking if Apache is running...
Apache is running. OK to proceed.

Accessing the FreePBX GUI...
Go to http://192.168.1.100 in your preferred web browser. OK to proceed.

Cleaning Asterisk logs...
Full, fail2ban, and root mail cleared. OK to proceed.

Clearing bash history...
Bash history cleared. OK to proceed.

WARNING: System will reboot in 60 seconds...

Press Enter to reboot now or Ctrl+C to cancel.

Attempting to delete IN1CLICK by 20tele.com: /tmp/IN1CLICK
IN1CLICK by 20tele.com removed successfully.

IN1CLICK completed in 14min 30sec.

Goodbye.
```

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
