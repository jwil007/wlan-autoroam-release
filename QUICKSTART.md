# wlan-autoroam - Quick Start

## Installation

1. Download the `wlan-autoroam` binary
2. Make it executable:
   ```bash
   chmod +x wlan-autoroam
   ```
3. Run with sudo:
   ```bash
   sudo ./wlan-autoroam
   ```

## First Run - Setup Wizard

The binary will automatically guide you through setup:

```
╔════════════════════════════════════════╗
║     wlan-autoroam Setup Wizard        ║
╚════════════════════════════════════════╝

ℹ Checking system dependencies...
✓ System dependencies found

Enter web UI username: admin
Enter web UI password: ********

Select wireless interface:
  1. wlan0
  2. wlan1
Select (1-2): 1

✓ Setup complete!

[+] Starting UI server on port 8443
 * Running on https://127.0.0.1:8443
```

Then:
1. Open browser to **https://localhost:8443**
2. Accept the self-signed certificate warning
3. Log in with your credentials
4. Start roaming tests!

## Usage

```bash
# Start server (default port 8443)
sudo ./wlan-autoroam

# Use custom port
sudo ./wlan-autoroam -p 9443

# Reconfigure (re-run wizard)
sudo ./wlan-autoroam --setup

# Help
./wlan-autoroam --help
```

## Requirements

- Linux with wireless support
- Root/sudo access
- System utilities: `wpa_cli`, `iw`, `journalctl`, `ip`

Install dependencies:
```bash
sudo apt install wireless-tools wpasupplicant iproute2 -y
```

## Config Files

All settings stored in `~/.config/wlan-autoroam/`:
- `.env` - Web credentials
- `user_prefs.json` - Interface defaults
- `ai_settings.json` - LLM config (optional)

Test data stored in `~/.local/share/wlan-autoroam/runs/`

## Need Help?

See [BINARY_USAGE.md](BINARY_USAGE.md) for detailed documentation.
