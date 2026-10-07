# mcp-manager — MCP Server Toggle Utility
**Purpose:** Toggle MCP servers via disabled flag, kill disabled servers by exe-stem, trigger opencode reload
**Source:** Tools/mcp_on_demand.ps1, Tools/mcp_json_edit.py
**Provenance:** PROJECT_MEMORY.md L-245 (MCP_ON_DEMAND)
**Tested:** Yes (2026-10-05, L-245)
**Sandbox-Safe:** Yes (reads/writes project opencode.json only)

## Usage
```powershell
# Presets: full / unity / device / min (+ solo/enable/disable/status/list)
.\mcp_on_demand.ps1 -Preset unity
.\mcp_on_demand.ps1 -Solo unity-mcp
.\mcp_on_demand.ps1 -Disable blender
.\mcp_on_demand.ps1 -Status
```

## Exit Codes
- 0: Success — config updated, servers toggled, opencode reload complete
- 1: Config file not found or invalid JSON
- 2: opencode reload failed
- 3: Process kill failed (stale servers remain)

## Protocol (per L-245)
1. Edit `opencode.json` disabled flags via mcp_json_edit.py (never inline PS JSON)
2. Kill ONLY disabled servers' processes by exe-stem match (never bare python/node/java)
3. `opencode reload` from project root — rebinds fresh session
4. Print BEFORE/AFTER census + `mcp list`
5. Assert: AFTER census procs <= 10 on unity preset AND list shows exactly unity-mcp + remotes

## Key Lessons (L-245)
- PS 5.1 `python -c $var` strips embedded double-quotes → all JSON edits via stable .py file
- First preset run may edit nothing (bug) yet kill 194 procs → reload respawns everything, zero harm
- RULE: Every preset run prints BEFORE/AFTER census + mcp list; run that kills without matching config edit is VOID unless post-reload list shows intended set