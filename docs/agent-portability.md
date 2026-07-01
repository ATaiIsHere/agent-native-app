# Agent Portability

An Agent-native App should not depend on one specific agent implementation.

The app should be installable by different personal agents or coding agents, such as:

- Hermes Agent
- Claude Code
- Codex
- Cursor-style agents
- custom MCP clients
- future personal agent runtimes

## Design goal

The app package should expose standard surfaces instead of assuming a specific agent runtime:

```text
Docker / Compose       -> reproducible runtime
HTTP API               -> conventional app integration
MCP server             -> agent tool/data interface
SKILL.md               -> agent guidance
README / RUNBOOK       -> human and agent operations
app.yaml               -> machine-readable app manifest
ai-logic-contract.yaml -> AI delegation points
```

Hermes may be one consumer. It should not be the only consumer.

## What belongs in the agent home

The agent home should contain lightweight agent context:

- skills
- config
- memory
- credentials or references to credential stores
- connector configuration

It should not become the default storage location for every app's runtime data.

## What belongs in the app boundary

The app boundary should contain app-owned state:

- database
- uploaded files
- caches
- generated runtime files
- migrations
- app logs, where appropriate
- backup/restore scripts

## Dependency direction

The app should not require a specific agent to exist inside its runtime.

Prefer:

```text
Personal Agent -> calls MCP/API -> App
```

Avoid:

```text
App -> embeds hardcoded Agent lifecycle -> specific agent runtime
```

The app may provide examples for Hermes, Claude Code, Codex, or other agents, but those should be integration recipes, not hard dependencies.

## Portable Skill design

A `SKILL.md` should describe domain behavior and tool usage without assuming one agent vendor whenever possible.

Good Skill guidance:

- what the app does
- when to use it
- what state to read first
- which actions are safe
- which actions require approval
- how to verify write-back
- how to recover from failure

Avoid hardcoding:

- one agent's private memory format
- one specific chat platform
- one local filesystem layout
- one vendor-only tool name, unless placed in an integration-specific appendix

## Why portability matters

If the app follows this pattern, the user can change their personal agent without rewriting the app.

The app remains a stable service. The personal agent remains replaceable.

That is the architectural boundary this pattern is trying to protect.
