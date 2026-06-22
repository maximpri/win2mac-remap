# ⌨️ win2mac-remap

[![macOS](https://img.shields.io/badge/macOS-14%2B-292e33?logo=apple&logoColor=white)](https://support.apple.com/macos)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> Plug a Windows keyboard into your Mac and instantly make it feel native: Win2Mac Remap maps the **Windows key → Option** and **Alt → Command**, so your Mac shortcuts work exactly the way you expect.

## ✨ Features

- **Zero dependencies** — uses macOS built-in `hidutil`
- **One‑liner install** — pipe from curl or download and run
- **Survives reboots** — installs a LaunchAgent that reapplies the remap
- **Self‑healing** — reapplies every 60 seconds (macOS can drop HID mappings)
- **Per‑device targeting** — remap only your external keyboard, not the built‑in one
- **Self‑updating** — `win2mac-remap update` refreshes the installed script

## 🚀 Quick Start

### One‑liner (recommended)

```bash
curl -fsSL https://raw.githubusercontent.com/maximpri/win2mac-remap/main/win2mac-remap | bash
```

### Manual install

```bash
curl -O https://raw.githubusercontent.com/maximpri/win2mac-remap/main/win2mac-remap
chmod +x win2mac-remap
./win2mac-remap install
```

After install, the command is available globally:

```bash
win2mac-remap status
```

### Install from source

```bash
git clone https://github.com/maximpri/win2mac-remap.git
cd win2mac-remap
./win2mac-remap install
```

## 🔄 What It Remaps

| Physical Windows Key | Becomes on Mac |
|----------------------|----------------|
| <kbd>⌘ Win</kbd> | <kbd>⌥ Option</kbd> |
| <kbd>Alt</kbd> | <kbd>⌘ Command</kbd> |

So <kbd>Alt</kbd> + <kbd>C</kbd> / <kbd>V</kbd> / <kbd>X</kbd> / <kbd>Z</kbd> on your Windows keyboard becomes <kbd>⌘ Command</kbd> + <kbd>C</kbd> / <kbd>V</kbd> / <kbd>X</kbd> / <kbd>Z</kbd> on Mac.

## 📖 Commands

| Command | Description |
|---------|-------------|
| `install` | Install script, LaunchAgent, and apply remap now |
| `update` | Update installed script and reload LaunchAgent |
| `apply` | Apply / re‑apply remap immediately |
| `status` | Show installation status and current `hidutil` mapping |
| `reset` | Clear current `hidutil` remap |
| `uninstall` | Remove LaunchAgent, clear remap, delete installed files |
| `devices` | List HID devices to find VendorID / ProductID |
| `help` | Show full help text |

## ⚙️ Options

```bash
# Target only a specific external keyboard
win2mac-remap install --match '{"VendorID":0x046D,"ProductID":0xC31C}'

# Change reapply interval (default: 60 seconds)
win2mac-remap install --interval 30

# Reduce output (silent mode)
win2mac-remap install --quiet
```

Use `win2mac-remap devices` to find your keyboard's VendorID and ProductID.

## ⚠️ Permissions

On **macOS Sonoma / Sequoia**, you may need to grant **Input Monitoring** permission to your terminal app:

```
System Settings → Privacy & Security → Input Monitoring
→ Enable Terminal.app (or iTerm2)
```

If `hidutil` fails, the script will print this reminder.

## 🧠 How It Works

1. Uses Apple's `hidutil property --set` to modify HID keyboard modifier mappings ([Apple TN2450](https://developer.apple.com/library/archive/technotes/tn2450/_index.html))
2. Installs itself to `~/.local/bin/win2mac-remap`
3. Creates a user LaunchAgent (`gui/$UID`) at `~/Library/LaunchAgents/com.user.win2mac-remap.plist`
4. The LaunchAgent runs `win2mac-remap apply --quiet` every 60 seconds and at login
5. No admin / `sudo` required — everything runs in user space

## 🗑️ Uninstall

```bash
win2mac-remap uninstall
```

This removes the LaunchAgent, clears the remap, and deletes all installed files.

## 📄 License

MIT — feel free to use, modify, and share.
