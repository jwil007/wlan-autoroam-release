# wlan-autoroam# wlan-autoroam



An automated Wi-Fi roaming analyzer with agentic AI capabilities. Test your wireless network's roaming performance across access points with detailed phase-by-phase metrics and AI-powered analysis.

An automated Wi-Fi roaming analyzer with agentic AI capabilities. Uses native Linux tools (iw, wpa_cli, wpa_supplicant, and journalctl) to scan and roam across BSSIDs in your ESS, measuring performance at each stage. An integrated AI assistant (RoamBot) can autonomously analyze results, compare test runs, and answer questions about your network's roaming behavior.

<img width="800" alt="wlan-autoroam web interface" src="https://github.com/user-attachments/assets/f8b3311d-4a26-4d29-a3e2-623856023649" />



> [!IMPORTANT]> [!IMPORTANT]

> **AI Features Require Your Own API Key**: RoamBot needs an LLM provider (OpenAI, Anthropic, OpenRouter, or local Ollama/LM Studio). Basic roaming tests work without AI. See [BINARY_USAGE.md](BINARY_USAGE.md) for configuration.> **AI Features Require Your Own API Key**: RoamBot needs an LLM provider (OpenAI, Anthropic, OpenRouter, or local Ollama/LM Studio). See [AI Analysis Setup](#ai-analysis-roambot) for configuration. Basic roaming tests work without AI.



## Features<img width="800" alt="10 0 10 58_8443_ (4)" src="https://github.com/user-attachments/assets/f8b3311d-4a26-4d29-a3e2-623856023649" />



* **🤖 Agentic AI Assistant (RoamBot)** - Natural language interface with autonomous tool usage. Ask questions, run tests, compare networks, and get insights without manual data wrangling. Supports OpenAI, Anthropic, OpenRouter, or local LLMs (Ollama/LM Studio).## Features

* **Mobility Score** - Easy-to-read score to evaluate your roaming readiness ([learn more](docs/MOBILITY_SCORE.md)).* **🤖 Agentic AI Assistant (RoamBot)** - Natural language interface with autonomous tool usage. Ask questions, run tests, compare networks, and get insights without manual data wrangling. Supports OpenAI, Anthropic, OpenRouter, or local LLMs (Ollama/LM Studio).

* **Multi-Run Analysis** - AI can autonomously fetch and compare multiple test runs, track performance trends, and identify patterns across networks.* **Mobility Score** - Easy to read score to evaluate your roaming readiness.

* **Interactive Web UI** - Modern interface with real-time test execution, AI chat, and result management.* **Multi-Run Analysis** - AI can autonomously fetch and compare multiple test runs, track performance trends, and identify patterns across networks.

* **Automatic AP Selection** - Intelligently sequences roams based on RSSI, security type, and network configuration.* **Interactive Web UI** - Modern interface with real-time test execution, AI chat, and result management.

* **Phase Timing Breakout** - Measures duration for auth, reassoc, EAP, and 4-way handshake from the client perspective.* **Automatic AP Selection** - Intelligently sequences roams based on RSSI, security type, and network configuration.

* **Failed Roam Diagnostics** - Auto-saves log snippets for failed roams with detailed error context.* **Phase Timing Breakout** - Measures duration for auth, reassoc, EAP, and 4-way handshake from the client perspective.

* **Save/Load Results** - Persist test runs with notes for historical comparison and trend analysis.* **Failed Roam Diagnostics** - Auto-saves log snippets for failed roams with detailed error context.

* **MCP Integration** - Model Context Protocol server for AI agent interactions ([learn more](docs/MCP_EXPLAINER.md)).* **Save/Load Results** - Persist test runs with notes for historical comparison and trend analysis.

* **REST API** - Programmatic access to all test and analysis functions (Beta).* **REST API** - Programmatic access to all test and analysis functions (Beta).

* **CLI Mode** - Headless operation for scripting and automation.

## Quick Start

## Quick Start

### Prerequisites

The easiest way to get started is with the automated setup script:

**System Requirements:**

- Linux (Debian/Ubuntu or similar)```bash

- Active Wi-Fi connection# Clone the repository

- Root/sudo access (required for wireless tools: `iw`, `wpa_cli`)git clone https://github.com/jwil007/wlan-autoroam.git

cd wlan-autoroam

**Your system needs:**

- `wpa_supplicant` and `wpa_cli`# Run the setup script (handles everything)

- `iw` (wireless tools)sudo ./autoroam.sh

- `journalctl` (systemd)```

- `ip` (iproute2)

On first run, the script will:

Most modern Linux distributions have these installed by default.- Prompt for web UI username and password

- Prompt to select your wireless interface (auto-detected)

### Installation- Generate a secure Flask secret key

- Create and configure `.env` file

1. **Download the binary** from the [latest release](../../releases/latest)- Check for required system utilities (wpa_cli, iw, journalctl)

- Install python3-venv if needed

2. **Make it executable:**- Create virtual environment and install dependencies

   ```bash- Launch the web UI at https://localhost:8443

   chmod +x wlan-autoroam

   ```On subsequent runs, it simply launches the application.



3. **Run the setup wizard:****Optional:** Use `-p` flag to specify a different port: `sudo ./autoroam.sh -p 10443`

   ```bash

   sudo ./wlan-autoroam

   ```

### Prerequisites

   On first run, the wizard will:This project is built around Linux Debian based distros. It uses iw, wpa_cli, and journalctl. Your Linux client needs to have an active Wi-Fi interface connected to an SSID. Make a note of your interface name, which you can get with `ip link`.

   - Check for required system utilities

   - Detect your wireless interface(s)**System packages required:**

   - Prompt for web UI username and password```bash

   - Generate secure configuration filessudo apt-get install python3-dev python3-venv libffi-dev

   - Launch the web UI at https://localhost:8443```



4. **Access the web interface:**> [!NOTE]

   - Open https://localhost:8443> - `python3-dev` and `libffi-dev` are required for building C extensions (especially on ARM systems like Raspberry Pi)

   - Login with your configured credentials> - On some systems, wpa_cli and iw require root. In general, running with sudo is recommended

   - Accept the self-signed certificate warning (or provide your own certs)



That's it! See [QUICKSTART.md](QUICKSTART.md) for your first roaming test.

#### Manual Setup

### Configuration Files

If you prefer manual setup:

After setup, your configuration lives in:

- **Config**: `~/.config/wlan-autoroam/````bash

  - `.env` - Web UI credentials# Install Python development packages

  - `user_prefs.json` - Default interface and RSSI thresholdsudo apt-get install python3-dev python3-venv libffi-dev

  - `certs/` - SSL certificates (replaceable)

  - `ai_settings.json` - AI provider configuration (via web UI)# Configure environment variables

cp .env.example .env

- **Data**: `~/.local/share/wlan-autoroam/`# Edit .env and set WEB_USER, WEB_PASS, and FLASK_SECRET

  - `runs/` - Test results and saved runs

  - `roam.log` - Application logs# Create and activate virtual environment

python3 -m venv venv

See [BINARY_USAGE.md](BINARY_USAGE.md) for detailed configuration options.venv/bin/pip install -r requirements.txt



## Documentation# Run the app (requires root for iw / wpa_cli)

sudo venv/bin/python3 start_autoroam_ui.py

- **[QUICKSTART.md](QUICKSTART.md)** - Run your first roaming test```

- **[BINARY_USAGE.md](BINARY_USAGE.md)** - Complete usage guide, configuration, and troubleshooting# Usage

- **[docs/MOBILITY_SCORE.md](docs/MOBILITY_SCORE.md)** - Understanding the mobility score## Web UI:

- **[docs/MCP_EXPLAINER.md](docs/MCP_EXPLAINER.md)** - Model Context Protocol integration

The UI is the primary interface - click Run Now to start a test, or just ask RoamBot to run one for you.

## Usage

1. Launch the web UI with `sudo ./autoroam.sh` (or manual setup above).

```bash2. Log in at https://localhost:8443 (or whatever port you set). To access from another device, use the host's IP (e.g. https://10.0.10.58:8443).

# Start the web UI (requires sudo for wireless tools)3. **Optional:** Set interface and min_rssi params in the top-right fields. Leave blank to use your saved preferences (configured during first-time setup). Click Run Now to start the test.

sudo ./wlan-autoroam  > [!NOTE]

> The cert for HTTPS is self signed, so you'll need to click through the browser warning. To replace the certs, edit `server.crt` and `server.key` in `webui/server/certs`.

# Specify a different port

sudo ./wlan-autoroam -p 10443### Saving and loading results

After running a roam cycle, save results to analyze later - otherwise they'll be flushed on the next test run.

# Force re-run the setup wizard

sudo ./wlan-autoroam --setupClick `💾 Save Results` to archive a run. Add optional notes in the modal for context.

```

Load previous results using the `Load Results...` dropdown. Results are labeled with SSID and timestamp - e.g. _Wilson-Corp (10/13/2025, 3:43:30 PM)_.

The web interface will be available at https://localhost:8443 (or your chosen port).

## AI Analysis (RoamBot)

## Support

RoamBot is an agentic AI assistant that can run tests, analyze results, compare networks, and answer questions about your Wi-Fi infrastructure. It uses the Model Context Protocol (MCP) to access tools and data autonomously.

**Issues and Questions:**

- Found a bug? [Open an issue](../../issues)**What makes it "agentic"?** RoamBot decides which actions to take based on your question. Ask to "run a test and compare it to yesterday's run" - it'll start the test, wait for completion, fetch the historical data, and provide a comparison. No button clicking required.

- Have a question? [Start a discussion](../../discussions)

### Setup

**Security Note:**

Always run wlan-autoroam with `sudo` as it requires root privileges to access wireless utilities (`iw`, `wpa_cli`). Config files are created with secure permissions (600/644).1. Click **AI Settings** (top bar icon) and configure your provider:

   - **OpenAI**: Get API key from https://platform.openai.com/api-keys. Endpoint: `https://api.openai.com/v1`.

## License   - **Anthropic**: Get API key from https://console.anthropic.com/. Endpoint: `https://api.anthropic.com/v1`.

   - **OpenRouter** (multi-model): Get API key from https://openrouter.ai/. Endpoint: `https://openrouter.ai/api/v1`. Supports Claude, GPT-4, Gemini, and 200+ models.

Licensed under the GNU GPL v3.0 - see [LICENSE](LICENSE) for details.   - **Ollama** (local): Install with `curl -fsSL https://ollama.ai/install.sh | sh`, pull a model (`ollama pull llama3.2`), set endpoint to `http://localhost:11434/v1`.

   - **LM Studio** (local): Download from https://lmstudio.ai/, start the server, set endpoint to `http://localhost:1234/v1`.

---

2. Save settings and start chatting. RoamBot will introduce itself.

**Built with:** Python, Flask, FastMCP, and the Model Context Protocol

**Token usage:** The AI panel header shows token consumption. Agentic mode can pull significant data when comparing runs - watch your usage carefully. Anthropic models (especially Claude Opus) provide the best analysis but are pricey. For free usage, go local with Ollama.

**Privacy:** Your credentials and test data are only sent when you request analysis. Use local providers (Ollama/LM Studio) for complete privacy.

**Customization:** Edit AI behavior in `autoroam/tools/prompt_engineering.py`.

> [!NOTE]
> AI settings (API key, endpoint, model) are stored locally in `webui/server/ai_settings.json`. Your preferred interface is saved in `webui/server/user_prefs.json` - tell RoamBot once and it remembers.

### Example Prompts

**Running tests:**
- "Run a roam test on wlp0s20f3"
- "Test my network and save the results"
- "Run a test with RSSI threshold of -70"

**Analysis:**
- "What were the failures in this test?"
- "Why did roam #3 take so long?"
- "Are there any configuration issues with these APs?"

**Multi-run comparison:**
- "Compare the last two Wilson-Corp runs"
- "Show me all IoT tests from this week"
- "Has performance improved since yesterday?"
- "Which network has better roaming performance?"
- "What's the trend in association times?"

**Network audit:**
- "Are all my APs configured consistently?"
- "Which APs are overloaded?"
- "Do I have co-channel interference?"

### REST API
Experimental REST API for programmatic access. Swagger docs at https://localhost:8443/api/docs (handy button in the UI after login).

API calls require `X-API-Key` header with the key from `webui/server/api_key.txt`.

## CLI:
`sudo venv/bin/python3 start_autoroam_cli.py`
 
#### Optional args:
 
  `-h, --help`          show this help message and exit
  
  `-i, --iface IFACE`   Wi-Fi interface to use. Default: wlan0
  
  `-r, --rssi RSSI `    Minimum RSSI filter. Default: -75
 

## Troubleshooting

### Installation Issues

**Error: `Python.h: No such file or directory` OR `ffi.h: No such file or directory`**

These errors occur when Python development headers or libffi headers aren't installed. The `cffi` package needs both to compile C extensions.

**Fix:**
```bash
# For default Python 3
sudo apt-get install python3-dev libffi-dev

# Or for specific Python version (e.g., 3.13)
sudo apt-get install python3.13-dev libffi-dev
```

Then retry: `venv/bin/pip install -r requirements.txt`

**Common on:** Raspberry Pi, ARM systems, minimal Debian installations.

---

**Error: `Permission denied` when running tests**

The tools require root access to manage wireless interfaces:
```bash
sudo ./autoroam.sh
# or
sudo venv/bin/python3 start_autoroam_ui.py
```

---

### AI Chat Issues

**Error: "⚠️ Error: AI chat failed. Please check your AI Settings."**

This is a generic error. To debug:

1. **Check Browser Console (F12)** - Look for detailed error messages in the console
2. **Expand "Technical Details"** - Click the dropdown in the error message to see backend details
3. **Common causes:**
   - **Invalid API key** - Verify your API key is correct in AI Settings
   - **Wrong endpoint** - Anthropic uses `https://api.anthropic.com/v1`, OpenAI uses `https://api.openai.com/v1`
   - **Rate limits** - You may have hit your provider's rate limit (wait and retry)
   - **Insufficient credits** - Your account may be out of credits (check provider dashboard)
   - **Model not available** - Some models require specific API access (try a different model)
   - **Network issues** - Check internet connection or firewall blocking API calls

**Debugging steps:**
```bash
# Check server logs for detailed errors
tail -f /path/to/server/logs

# Test API key manually
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.anthropic.com/v1/models

# Restart the server
sudo ./autoroam.sh
```
### Screenshots
#### Agentic AI and graphical analysis
<img width="1239" height="1335" alt="10 0 10 58_8443_ (4)" src="https://github.com/user-attachments/assets/f8b3311d-4a26-4d29-a3e2-623856023649" />

#### Mobility Score
<img width="413" height="419" alt="mobilityscorescreenshot" src="https://github.com/user-attachments/assets/6fa5691c-3522-48e2-9f4e-749c7105b806" />

#### AP list and per roam details
<img width="1234" height="1044" alt="AP list with roam deetz" src="https://github.com/user-attachments/assets/4a13dd40-afdb-4d36-a1b2-a2a6660825aa" />








