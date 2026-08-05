# Xcode MCP Standard

Use `xcode-mcp` to configure, verify, troubleshoot, or remove access to Apple's built-in Xcode MCP bridge for the current AI agent.

## Scope

This standard covers Apple's `xcrun mcpbridge` flow for Xcode 26.3 or newer. It is an agent tooling setup workflow, not a product specification workflow:

- Do configure MCP client settings for the selected AI agent.
- Do help the user enable Xcode's external-agent access.
- Do verify that the agent can reach Xcode tools.
- Do not initialize `.open-canal/` or write requirement, design, development, test, or version artifacts.
- Do not install third-party Xcode MCP servers unless Apple's built-in bridge is unavailable and the user explicitly approves a fallback.

## Command Parameter

Use one command parameter:

- `configure` - add the Xcode MCP server to the selected AI agent.
- `verify` - confirm that the selected AI agent can list and connect to the Xcode MCP server.
- `remove` - remove the Xcode MCP server from the selected AI agent.
- `troubleshoot` - diagnose failed setup, connection, tool discovery, build, or test execution.

If the command is missing, infer it from the user's request. Ask only when the intent cannot be inferred.

## Preconditions

Before changing configuration, check what can be checked locally:

```bash
sw_vers
xcodebuild -version
xcode-select -p
xcrun --find mcpbridge
```

Required conditions:

- macOS with Xcode 26.3 or newer selected by `xcode-select`;
- `xcrun --find mcpbridge` resolves;
- Xcode is open with the target project or workspace loaded before connection verification;
- Xcode Settings -> Intelligence -> Model Context Protocol allows external agents to use Xcode tools;
- the selected AI agent supports MCP over stdio.

If Xcode is not open, configuration can still be written, but verification must report that the user needs to open Xcode and the target project before testing the connection.

## Agent Selection

Configure the current AI agent by default.

Selection priority:

1. Explicit user choice, such as "configure Cursor".
2. Active runtime identity, such as Codex, Claude Code, Cursor, OpenCode, or Crush.
3. Existing MCP config files or available CLI commands.
4. Ask a concise clarification question.

When running inside Codex or Codex App and the user does not name another agent, select Codex.

## Server Naming

Use a stable server name and avoid duplicates:

- Prefer `xcode` for CLI-based agents.
- Prefer `xcode-tools` for Cursor when creating a new Cursor entry.
- If an Xcode MCP server already exists under another name, update or verify that existing entry instead of adding a duplicate.

The server command is always:

```bash
xcrun mcpbridge
```

For JSON-based MCP clients, represent it as:

```json
{
  "command": "xcrun",
  "args": ["mcpbridge"]
}
```

## Configure

### Codex CLI / Codex App

Use Codex when the current agent is Codex and the `codex` command is available:

```bash
codex mcp add xcode -- xcrun mcpbridge
codex mcp list
```

If the `codex` command is unavailable in Codex App, report that the MCP config cannot be updated through the CLI from this environment and fall back to the current app's documented MCP configuration surface if one is available. Do not invent an undocumented config path.

### Claude Code

Use Claude Code when the current agent is Claude Code:

```bash
claude mcp add --transport stdio xcode -- xcrun mcpbridge
claude mcp list
```

### Cursor

Use Cursor when the user names Cursor or the current agent is Cursor.

Preferred UI path:

1. Open Cursor Settings.
2. Open Features -> MCP.
3. Add a new MCP server.
4. Set transport to `stdio`.
5. Set name to `xcode-tools`.
6. Set command to `xcrun mcpbridge`.

JSON path:

- Edit `~/.cursor/mcp.json`.
- Parse existing JSON and preserve unrelated servers.
- Add or update:

```json
{
  "mcpServers": {
    "xcode-tools": {
      "command": "xcrun",
      "args": ["mcpbridge"]
    }
  }
}
```

If the file contains invalid JSON, stop and report the parse error instead of overwriting it.

### OpenCode, Crush, and Other MCP Clients

For agents without a stable command documented in this standard:

1. Inspect the agent's local help, documentation, or existing MCP configuration.
2. Add a stdio MCP server named `xcode` or `xcode-tools`.
3. Use command `xcrun` and args `["mcpbridge"]`.
4. Report the exact config path or command used.

Do not guess destructive edits to global configuration. If the agent's MCP config shape is not discoverable, ask the user for the target config path or use the agent's UI.

## Verify

Verification must include both registration and connection when possible:

1. Confirm the selected agent lists the server (`codex mcp list`, `claude mcp list`, Cursor MCP UI, or equivalent).
2. Confirm Xcode is open with the target project or workspace.
3. Ask the user to approve Xcode's external-agent prompt if it appears.
4. Use the selected agent's MCP tools to inspect Xcode, such as listing projects, schemes, build settings, or tests.
5. For build/test requests, run the smallest relevant build or test target and report structured diagnostics.

If connection verification cannot run because Xcode is closed or awaiting user approval, report configuration as written but connection as unverified.

## Troubleshooting

- `xcrun: error: unable to find utility "mcpbridge"`: verify Xcode 26.3 or newer is installed and selected with `xcode-select`.
- Server is listed but tools are unavailable: open Xcode, load the project, enable Settings -> Intelligence -> Model Context Protocol, and approve the connection prompt.
- Agent connects to the wrong Xcode project: bring the intended Xcode window/project to the foreground, or close unrelated Xcode workspaces and retry.
- JSON config breaks the agent: restore the previous JSON content, validate syntax, and re-add only the Xcode server entry.
- Multiple agents compete for the same project: disconnect unused agents or avoid multiple simultaneous `mcpbridge` instances against the same Xcode project.
- `structuredContent` or schema errors on early Xcode 26.3 prerelease builds: upgrade Xcode before adding wrapper scripts. Use a wrapper only when the user explicitly requests a temporary workaround.

## Remove

For CLI agents, remove the server with the agent's MCP remove command after checking the actual server name:

```bash
codex mcp list
codex mcp remove xcode
```

```bash
claude mcp list
claude mcp remove xcode
```

For Cursor or JSON-based clients, delete only the Xcode MCP server entry from the config. Preserve unrelated `mcpServers` entries and verify the JSON remains valid.

After removal, report whether the agent still lists any Xcode MCP server aliases.

## Output

Return a concise report with:

- selected command parameter;
- selected AI agent;
- Xcode version and selected developer directory;
- server name;
- command run or config path edited;
- registration verification result;
- connection verification result;
- manual user actions still required in Xcode;
- any fallback or unresolved blocker.
