# wlan-autoroam Binary - Usage Guide

## Overview

The `wlan-autoroam` binary is a self-contained executable that includes:
- Web UI server (Flask)
- MCP server for AI integrations
- Complete roaming test framework
- Guided setup wizard

## First Run

On the **first run**, the binary will automatically launch a **setup wizard** that guides you through:

1. **Dependency Check**: Verifies required system utilities (`wpa_cli`, `iw`, `journalctl`, `ip`)
2. **Permission Validation**: Ensures you're running with `sudo` (required for wireless operations)
3. **Web Credentials**: Prompts for username/password for the web UI
4. **Interface Selection**: Helps you choose the default wireless interface
5. **Configuration Save**: Stores settings in `~/.config/wlan-autoroam/`

### Running the Setup Wizard

```bash
# First-time run (with sudo for wireless access)
sudo ./wlan-autoroam

# The wizard will guide you through:
# ✓ System dependency checks
# ✓ Web UI username/password setup
# ✓ Wireless interface selection
# ✓ Configuration file creation
```

### Example Setup Session

```
╔════════════════════════════════════════╗
║     wlan-autoroam Setup Wizard        ║
╚════════════════════════════════════════╝

ℹ Running as: root (via sudo from user: jacob)
ℹ Checking system dependencies...
✓ System dependencies found
✓ Wireless interface(s) detected: wlan0, wlan1

═══════════════════════════════════════
  Web UI Credentials Setup
═══════════════════════════════════════

Enter web UI username: admin
Enter web UI password: ********
ℹ Generating Flask secret key...
✓ Web credentials configured

═══════════════════════════════════════
  Wireless Interface Selection
═══════════════════════════════════════

ℹ Detecting available wireless interfaces...
Found multiple wireless interfaces:

  1. wlan0
  2. wlan1

Select interface (1-2): 1
✓ Default interface set to: wlan0
✓ Interface preference saved

═══════════════════════════════════════
✓ Setup complete!

ℹ Configuration saved to:
  /home/jacob/.config/wlan-autoroam/.env
  /home/jacob/.config/wlan-autoroam/user_prefs.json

[+] Starting UI server on port 8443
 * Running on https://127.0.0.1:8443
```

## Subsequent Runs

After initial setup, the binary **skips the wizard** and starts directly:

```bash
sudo ./wlan-autoroam

# Output:
# [+] Starting UI server on port 8443
# [+] MCP server will be spawned automatically by Flask
# * Running on https://127.0.0.1:8443
```

## Command-Line Options

```bash
# Run with custom port
sudo ./wlan-autoroam -p 9443

# Force re-run setup wizard (reconfigure)
sudo ./wlan-autoroam --setup

# Skip setup wizard (for testing)
sudo ./wlan-autoroam --skip-setup

# Show help
./wlan-autoroam --help
```

## Configuration Files

All configuration is stored in `~/.config/wlan-autoroam/`:

```
~/.config/wlan-autoroam/
├── .env                  # Web credentials, Flask secret
├── ai_settings.json      # AI/LLM provider config (optional)
├── api_key.txt           # API key for external access
├── user_prefs.json       # Default interface, RSSI threshold
└── certs/                # SSL certificates
    ├── server.crt
    └── server.key
```

### Editing Configuration

You can manually edit config files after setup:

```bash
# Edit web credentials or add LLM settings
nano ~/.config/wlan-autoroam/.env

# Edit default interface or RSSI threshold
nano ~/.config/wlan-autoroam/user_prefs.json

# Or use the web UI:
# Settings → AI Settings (for LLM config)
# Start Roam Test → Interface dropdown (for defaults)
```

## Data Storage

Test run data is stored in `~/.local/share/wlan-autoroam/`:

```
~/.local/share/wlan-autoroam/
└── runs/
    ├── 2025-10-23T13-19-39_Wilson-Corp/
    │   ├── cycle_summary.json
    │   ├── metadata.json
    │   ├── roam_debug.log
    │   └── failed_roams/
    └── ...
```

## System Requirements

### Required Utilities
- `wpa_cli` - WPA supplicant control
- `iw` - Wireless configuration
- `journalctl` - System journal access
- `ip` - Network interface control

Install on Debian/Ubuntu:
```bash
sudo apt install wireless-tools wpasupplicant iproute2 -y
```

### Permissions
- **Root/sudo required** for wireless operations (`iw`, `wpa_cli`)
- Binary must be run with `sudo ./wlan-autoroam`

## Troubleshooting

### "Missing required utilities" Error

```bash
sudo apt install wireless-tools wpasupplicant iproute2 -y
```

### "This application requires root privileges" Error

```bash
# Use sudo:
sudo ./wlan-autoroam

# NOT:
./wlan-autoroam  # ✗ Will fail
```

### "No wireless interfaces detected" Warning

The setup wizard will let you continue, but you'll need to manually configure the interface later via the Web UI or by editing `~/.config/wlan-autoroam/user_prefs.json`.

### Reset Configuration

To start fresh with the setup wizard:

```bash
# Option 1: Force re-run wizard
sudo ./wlan-autoroam --setup

# Option 2: Delete config and re-run
rm -rf ~/.config/wlan-autoroam/
sudo ./wlan-autoroam
```

### Web UI Not Accessible

Check that:
1. Server started successfully (look for "Running on https://...")
2. You're accessing the correct URL: `https://localhost:8443`
3. You're using **https** (not http)
4. You accept the self-signed certificate warning
5. You're using credentials from `.env` file

## Accessing the Web UI

1. Start the binary with `sudo ./wlan-autoroam`
2. Open browser to `https://localhost:8443`
3. Accept the self-signed certificate warning
4. Log in with credentials from setup wizard
5. Start roaming tests!

## Differences from Source Mode

| Feature | Binary Mode | Source Mode (autoroam.sh) |
|---------|-------------|---------------------------|
| Setup | Interactive wizard | Script with prompts |
| Config location | `~/.config/wlan-autoroam/` | Project directory |
| Dependencies | Check only system utils | Also checks Python/venv |
| Roam execution | In-process threading | Subprocess |
| Portability | Single file | Requires Python env |

## Building from Source

To build the binary yourself:

```bash
cd wlan-autoroam/
./build.sh

# Binary will be in dist/wlan-autoroam
```

## Security Notes

- `.env` file is created with `600` permissions (owner read/write only)
- `api_key.txt` is auto-generated for API access
- SSL certificates are self-signed (replace in `~/.config/wlan-autoroam/certs/` for production)
- Never commit or share your `.env` or `api_key.txt` files

## Advanced Usage

### External API Access

The binary exposes a REST API with API key authentication:

```bash
# Get API key
API_KEY=$(cat ~/.config/wlan-autoroam/api_key.txt)

# Make API request
curl -sk -H "X-API-Key: $API_KEY" https://localhost:8443/api/latest_cycle_summary
```

### Custom SSL Certificates

Replace self-signed certs with your own:

```bash
# Generate new certs
openssl req -x509 -newkey rsa:4096 \
  -keyout ~/.config/wlan-autoroam/certs/server.key \
  -out ~/.config/wlan-autoroam/certs/server.crt \
  -days 365 -nodes \
  -subj '/CN=yourdomain.com'

# Restart binary
```

## Support

For issues, feature requests, or questions:
- GitHub: https://github.com/jwil007/wlan-autoroam
- Open an issue with your system info and error messages
