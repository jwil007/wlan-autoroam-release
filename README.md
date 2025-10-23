# wlan-autoroam

An automated Wi-Fi roaming analyzer with agentic AI capabilities. Test your wireless network's roaming performance across access points with detailed phase-by-phase metrics and AI-powered analysis.

<img width="800" alt="wlan-autoroam web interface" src="https://github.com/user-attachments/assets/f8b3311d-4a26-4d29-a3e2-623856023649" />

> [!IMPORTANT]
> **AI Features Require Your Own API Key**: RoamBot needs an LLM provider (OpenAI, Anthropic, OpenRouter, or local Ollama/LM Studio). Basic roaming tests work without AI. See [BINARY_USAGE.md](BINARY_USAGE.md) for configuration.

## Features

* **🤖 Agentic AI Assistant (RoamBot)** - Natural language interface with autonomous tool usage. Ask questions, run tests, compare networks, and get insights without manual data wrangling. Supports OpenAI, Anthropic, OpenRouter, or local LLMs (Ollama/LM Studio).
* **Mobility Score** - Easy-to-read score to evaluate your roaming readiness ([learn more](docs/MOBILITY_SCORE.md)).
* **Multi-Run Analysis** - AI can autonomously fetch and compare multiple test runs, track performance trends, and identify patterns across networks.
* **Interactive Web UI** - Modern interface with real-time test execution, AI chat, and result management.
* **Automatic AP Selection** - Intelligently sequences roams based on RSSI, security type, and network configuration.
* **Phase Timing Breakout** - Measures duration for auth, reassoc, EAP, and 4-way handshake from the client perspective.
* **Failed Roam Diagnostics** - Auto-saves log snippets for failed roams with detailed error context.
* **Save/Load Results** - Persist test runs with notes for historical comparison and trend analysis.
* **MCP Integration** - Model Context Protocol server for AI agent interactions ([learn more](docs/MCP_EXPLAINER.md)).
* **REST API** - Programmatic access to all test and analysis functions (Beta).

## Quick Start

### Prerequisites

**System Requirements:**
- Linux (Debian/Ubuntu or similar)
- Active Wi-Fi connection
- Root/sudo access (required for wireless tools: `iw`, `wpa_cli`)

**Your system needs:**
- `wpa_supplicant` and `wpa_cli`
- `iw` (wireless tools)
- `journalctl` (systemd)
- `ip` (iproute2)

Most modern Linux distributions have these installed by default.

### Installation

1. **Download the binary** from the [latest release](../../releases/latest)

2. **Make it executable:**
   ```bash
   chmod +x wlan-autoroam
   ```

3. **Run the setup wizard:**
   ```bash
   sudo ./wlan-autoroam
   ```

   On first run, the wizard will:
   - Check for required system utilities
   - Detect your wireless interface(s)
   - Prompt for web UI username and password
   - Generate secure configuration files
   - Launch the web UI at https://localhost:8443

4. **Access the web interface:**
   - Open https://localhost:8443
   - Login with your configured credentials
   - Accept the self-signed certificate warning (or provide your own certs)

That's it! See [QUICKSTART.md](QUICKSTART.md) for your first roaming test.

### Configuration Files

After setup, your configuration lives in:
- **Config**: `~/.config/wlan-autoroam/`
  - `.env` - Web UI credentials
  - `user_prefs.json` - Default interface and RSSI threshold
  - `certs/` - SSL certificates (replaceable)
  - `ai_settings.json` - AI provider configuration (via web UI)

- **Data**: `~/.local/share/wlan-autoroam/`
  - `runs/` - Test results and saved runs
  - `roam.log` - Application logs

See [BINARY_USAGE.md](BINARY_USAGE.md) for detailed configuration options.

## Documentation

- **[QUICKSTART.md](QUICKSTART.md)** - Run your first roaming test
- **[BINARY_USAGE.md](BINARY_USAGE.md)** - Complete usage guide, configuration, and troubleshooting
- **[docs/MOBILITY_SCORE.md](docs/MOBILITY_SCORE.md)** - Understanding the mobility score
- **[docs/MCP_EXPLAINER.md](docs/MCP_EXPLAINER.md)** - Model Context Protocol integration

## Usage

```bash
# Start the web UI (requires sudo for wireless tools)
sudo ./wlan-autoroam

# Specify a different port
sudo ./wlan-autoroam -p 10443

# Force re-run the setup wizard
sudo ./wlan-autoroam --setup
```

The web interface will be available at https://localhost:8443 (or your chosen port).

## Support

**Issues and Questions:**
- Found a bug? [Open an issue](../../issues)
- Have a question? [Start a discussion](../../discussions)

**Security Note:**
Always run wlan-autoroam with `sudo` as it requires root privileges to access wireless utilities (`iw`, `wpa_cli`). Config files are created with secure permissions (600/644).

## License

Licensed under the GNU GPL v3.0 - see [LICENSE](LICENSE) for details.

---

**Built with:** Python, Flask, FastMCP, and the Model Context Protocol
