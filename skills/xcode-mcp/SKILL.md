---
name: xcode-mcp
description: Use when the user wants to configure, verify, troubleshoot, or remove Xcode MCP / xcrun mcpbridge access for the current AI agent, including Codex, Claude Code, Cursor, OpenCode, Crush, or another MCP-compatible client.
---

# Xcode MCP

## Command Parameter

Use this skill with one command parameter:

- `configure` - register Xcode's MCP bridge with the selected AI agent.
- `verify` - check that the agent can see and connect to the Xcode MCP server.
- `remove` - remove the Xcode MCP server registration from the selected AI agent.
- `troubleshoot` - diagnose a failed Xcode MCP setup.

If the user does not provide a command, infer `configure` when they ask to set up or enable Xcode MCP, infer `verify` when they ask whether it works, infer `remove` when they ask to disconnect or uninstall it, and infer `troubleshoot` when they report connection, tool, build, or test failures.

## Load Standard

Read `references/standards/xcode-mcp.md` first. If the target project has `.open-canal/standards/xcode-mcp.md`, use that project-local standard as an override for repository-specific policy.

## Scope

This skill configures the AI agent's MCP client to use Apple's built-in Xcode MCP bridge. It does not initialize an open-canal specification vault, create product planning artifacts, or install third-party Xcode MCP servers unless the standard's fallback conditions apply.

## Current Agent Selection

Configure the current AI agent by default. Use this priority order:

1. The agent explicitly named by the user.
2. The active runtime when it is clear, such as Codex, Claude Code, Cursor, OpenCode, or Crush.
3. Existing agent config files or installed CLI commands.
4. A short clarification question when multiple agents are plausible.

Do not configure a different agent just because its command examples are easier.

## Workflow

1. Identify the command parameter and selected AI agent.
2. Run the preflight checks from `references/standards/xcode-mcp.md`.
3. Apply the agent-specific MCP registration or removal rule.
4. Verify the server is listed by the agent.
5. If Xcode is running with a project open, verify an MCP connection by asking the agent to inspect the Xcode project, list schemes, build, or run tests as appropriate.
6. Report the selected agent, command used or config path edited, verification result, and any required manual action in Xcode.

## Safety

Before editing global agent configuration files, inspect existing content and preserve unrelated MCP servers. Do not overwrite JSON by string concatenation. Do not leave a manually launched `xcrun mcpbridge` process running when the MCP client is supposed to manage stdio lifecycle.
