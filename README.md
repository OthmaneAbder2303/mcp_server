# MCP Terminal Server

A lightweight **Model Context Protocol (MCP)** server that allows Claude Desktop (or any MCP client) to interact with your terminal in a controlled and safe environment.

## Purpose
This server exposes a `run_command` tool, enabling an AI assistant to execute shell commands within a specific workspace directory (`~/mcp/workspace`). It bridges the gap between AI reasoning and local file/system operations.

## Key Features
* **Isolated Execution:** Commands run by default in a dedicated workspace to prevent accidental system-wide changes.
* **Full Output Capture:** Captures both standard output (`stdout`) and errors (`stderr`) for precise AI feedback.
* **Built with FastMCP:** Leverages the latest MCP frameworks for high performance and easy extensibility.

## Project Structure
* `terminal_server.py`: The core MCP server logic.
* `pyproject.toml`: Dependency management (Python 3.13+, `mcp[cli]`).
* `setup_guide/`: **[Detailed Installation & Setup Guide](./setup_guide/mcp-terminal-server.md)**.

## Quick Start
For a step-by-step walkthrough on how to install, test, and connect this server to Claude Desktop, please refer to our dedicated guide:

👉 **[Read the Setup Guide](./setup_guide/mcp-terminal-server.md)**

## Configuration Snippet
Once your environment is ready, your `claude_desktop_config.json` will look similar to this:

```json
{
  "mcpServers": {
    "terminal": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/terminal_server",
        "run",
        "terminal_server.py"
      ]
    }
  }
}
```

## Security Note

> [!IMPORTANT]
> This server allows for system command execution. While scoped to a specific directory, always ensure the AI is operating on non-sensitive data.

<div align="center">

**⭐ Star this repo if you find it helpful!**


</div>
