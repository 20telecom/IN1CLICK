# IN1CLICK — Concurrency Count [1.0.0]

## Overview

**IN1CLICK** is a streamlined, automated installer script designed to quickly and reliably set up **Concurrency Count** — a powerful tool for analyzing call concurrency on FreePBX and PBXact systems.

Concurrency Count calculates the maximum number of concurrent calls on your telephony infrastructure, broken down by SIP trunks, extensions, or groups, helping you monitor system load and optimise capacity.

The IN1CLICK installer automates all necessary setup steps, making it easy for sysadmins and engineers to get started without manual configuration.

---

## Features

- **Automated Credentials Configuration:**  
  IN1CLICK checks for the presence of `/root/.my.cnf`, a secure MySQL client configuration file storing FreePBX database credentials. If missing, it extracts the required credentials from `/etc/freepbx.conf` and creates the `.my.cnf` file with strict permissions to ensure safe database access.

- **Dependency Management:**  
  It verifies that the `wget` utility is installed, installing it via the system’s package manager (`yum` for CentOS/RHEL or `apt` for Debian/Ubuntu) if absent, guaranteeing the ability to download required files.

- **Safe and Idempotent Installation:**  
  If the `concurrency-count` tool is already installed at `/usr/local/bin/concurrency-count`, IN1CLICK prompts the user to confirm whether to overwrite the existing version, preventing accidental loss of custom modifications.

- **Latest Version Retrieval:**  
  Downloads the most recent version of the `concurrency-count` script directly from the official 20tele.com GitHub repository (20telecom), ensuring you always get the newest features and fixes.

- **Immediate Verification:**  
  Automatically runs `concurrency-count` after installation to verify successful deployment and demonstrate usage with the welcome message and mode selection prompt.

- **Robust Error Handling and Cleanup:**  
  Uses Bash’s `set -e` option and a `trap` to detect errors at any step, cleaning up partially installed files (such as `.my.cnf` or the tool) and providing clear user guidance for troubleshooting.

- **Self-Removing Installer:**  
  Deletes the `IN1CLICK` installer script upon successful completion to keep the system clean and prevent clutter.

- **Minimal User Interaction:**  
  Aside from the overwrite confirmation prompt, the installer requires no additional input, enabling seamless automation or unattended installation if desired.

- **Update Behavior:**  
  Re-running IN1CLICK on a system with an existing Concurrency Count installation will prompt for overwrite. Confirming with “y” will download and install the latest version, keeping your installation up to date.

---

## Requirements

Before running IN1CLICK, ensure:

- **Root or sudo access:** Required to create files in `/root` and install binaries in `/usr/local/bin`.  
- **PHP installed:** The installer uses PHP to parse FreePBX configuration files; ensure `php` is installed and accessible in your system’s PATH (`php -v` to check).  
- **Internet access:** Needed to download the installer script and the `concurrency-count` tool.  
- **Supported OS:** Tested on CentOS 7 and Debian 12, but should work on similar Linux distributions with Bash, PHP, and standard package managers.  

---

## Installation Behavior and File Management

- The installer carefully **checks for the presence of `/root/.my.cnf`** before attempting to create it.  
- If `.my.cnf` already exists, the installer **will not overwrite or remove** this file, preserving any existing configurations you have.  
- If `.my.cnf` is created by the installer during this run, it will be removed if the installation fails, preventing orphaned or partial credential files.  
- Similarly, the `concurrency-count` script will be removed on failure to avoid incomplete or corrupted installations.  
- The installer script (`IN1CLICK`) **deletes itself on successful completion** to keep the system tidy.  
- This behavior ensures **no unintended data loss** or configuration changes occur during installation.

---

## Usage

### One-Click Installation and Run

Run the following command on your PBX server terminal to download, install, and launch the tool in a single step:

```bash
wget https://raw.githubusercontent.com/20telecom/IN1CLICK/main/concurrency-count -O /tmp/IN1CLICK && chmod +x /tmp/IN1CLICK && /tmp/IN1CLICK

## Support

If you need help, email support@20tele.com or open a ticket at https://support.20tele.com

---

## License

GNU General Public License v3.0

---
