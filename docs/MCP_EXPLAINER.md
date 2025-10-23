# MCP Explainer

Quick guide to how this project uses the Model Context Protocol.

## The Architecture

**MCP Server** = Dumb tools (no LLM, just functions)  
**MCP Client** = Smart orchestrator (calls LLM, manages agentic loop)  
**Flask** = HTTP wrapper  

```
Browser → Flask → MCP Client (+ LLM) → MCP Server (tools)
```

## How It Works

1. User asks: "Run a test on Wilson-Corp and analyze it"
2. Flask passes question to MCP Client
3. Client calls LLM with question + available tools
4. LLM responds: "Use start_roam tool"
5. Client calls MCP Server → `start_roam("Wilson-Corp")`
6. Server returns: `{"status": "started", "run_dir": "..."}`
7. Client feeds result back to LLM
8. LLM: "Now use wait_for_roam_completion"
9. (continues iterating up to 10 times)
10. LLM provides final answer
11. Client returns to Flask → Browser

## Available Tools

**Test Management:**
- `start_roam(ssid)` - Start test
- `wait_for_roam_completion(run_dir)` - Wait for test to finish

**Data Access:**
- `get_latest_summary()` - Most recent test
- `get_current_roam_data(run_dir)` - Load specific test
- `get_mobility_score(run_dir)` - Calculate network quality score (0-100)
- `list_runs_by_ssid(ssid)` - Find tests for SSID
- `compare_roam_runs(run_dirs)` - Compare tests

**UI:**
- `notify_ui_load_results(run_dir)` - Auto-refresh browser

## Adding a Tool

In `webui/server/fastmcp_server.py`:

```python
@mcp.tool()
def your_tool(param: str) -> dict:
    """What this tool does."""
    result = do_something(param)
    return {"status": "success", "data": result}
```

Done! The LLM automatically discovers it.

## Configuration

**AI Settings:** `webui/server/ai_settings.json`
```json
{
  "api_key": "sk-...",
  "model": "claude-3-5-sonnet-20241022",
  "temperature": 0.2
}
```

**Token optimization:** `MCP_FULL_TOOL_RESULTS=false` (saves ~45% tokens)

## Key Files

- `webui/server/fastmcp_server.py` - MCP tools (533 lines)
- `webui/server/mcp_client.py` - Agentic client (586 lines)
- `webui/server/app.py` - Flask routes (943 lines)

## Why This Way?

✅ LLM decides workflow dynamically  
✅ Tools are composable  
✅ Easy to add new capabilities  
✅ Clean separation of concerns  

That's it!
