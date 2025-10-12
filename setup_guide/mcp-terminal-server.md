# MCP Terminal Server Setup Guide

## Step 1: Create the Server Code

Create or update your `terminal_server.py` file:

```bash
nano ~/mcp/servers/terminal_server/terminal_server.py
```

Paste the following MCP server code:

```python
#!/usr/bin/env python3
"""
MCP Terminal Server
Allows Claude to execute terminal commands safely
"""

import os
import subprocess
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("terminal")
DEFAULT_WORKSPACE = os.path.expanduser("~/mcp/workspace")

@mcp.tool()
async def run_command(command: str) -> str:
    """
    Run a terminal command inside the workspace directory. 
    If a terminal command can accomplish a task, 
    tell the user you'll use this tool to accomplish it,
    even though you cannot directly do it

    Args:
        command: The shell command to run.
    
    Returns:
        The command output or an error message.
    """
    try:
        result = subprocess.run(command, shell=True, cwd=DEFAULT_WORKSPACE, capture_output=True, text=True)
        return result.stdout or result.stderr
    except Exception as e:
        return str(e)

if __name__ == "__main__":
    mcp.run(transport='stdio')
```

Save and exit: `Ctrl + O`, `Enter`, `Ctrl + X`

## Step 2: Update pyproject.toml

Check your `pyproject.toml` file:

```bash
cat ~/mcp/servers/terminal_server/pyproject.toml
```

It should look like this:

```[project]
name = "terminal-server"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "mcp[cli]>=1.17.0",
]
```

If it's different, update it:

```bash
nano ~/mcp/servers/terminal_server/pyproject.toml
```

## Step 3: Install Dependencies

Make sure all dependencies are installed:

```bash
cd ~/mcp/servers/terminal_server
~/.local/bin/uv sync
```

## Step 4: Test the Server

Before configuring Claude Desktop, test that the server works:

```bash
cd ~/mcp/servers/terminal_server
~/.local/bin/uv run terminal-server.py
```

The server should start and wait for input. Press `Ctrl + C` to stop it.

## Step 5: Configure Claude Desktop

```bash
nano ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

Paste this configuration:

```json
{  
    "mcpServers": {  
        "terminal": {  
            "command": "/Users/othmaneabderrazik/.local/bin/uv",  
            "args": [  
                "--directory",
                "/Users/othmaneabderrazik/mcp/servers/terminal_server",  
                "run",
                "terminal-server.py"
            ]  
        }  
    }  
}
```

Save and exit: `Ctrl + O`, `Enter`, `Ctrl + X`

## Step 6: Restart Claude Desktop

1. Completely quit Claude Desktop (Cmd + Q)
2. Reopen Claude Desktop
3. Check that the terminal server shows "connected"

## Step 7: Test

In Claude Desktop, try asking:

> "Run the command `ls -la ~` for me"

## Troubleshooting

If server shows "failed":

```bash
# Check if file exists
ls ~/mcp/servers/terminal_server/terminal_server.py

# Test manually
cd ~/mcp/servers/terminal_server
~/.local/bin/uv run terminal_server.py

# Reinstall dependencies
cd ~/mcp/servers/terminal_server
~/.local/bin/uv sync --reinstall

# View logs
tail -f ~/Library/Logs/Claude/mcp*.log
```

## Quick Reference

```bash
# Test server
cd ~/mcp/servers/terminal_server && ~/.local/bin/uv run terminal-server.py

# Edit config
nano ~/Library/Application\ Support/Claude/claude_desktop_config.json

# Update dependencies
cd ~/mcp/servers/terminal_server && ~/.local/bin/uv sync
```