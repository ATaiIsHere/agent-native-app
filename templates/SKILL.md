---
name: my-agent-native-app
description: Use when the user wants to operate, review, or delegate AI logic for My Agent-native App.
---

# My Agent-native App

## What this app does

Describe the app in one paragraph.

## When to use

Use this app when:

- condition 1
- condition 2

Do not use this app when:

- condition 1
- condition 2

## Operating rules

1. Read current app state before making decisions.
2. Prefer safe read-only inspection before write actions.
3. Follow the AI Logic Contract for decision points.
4. Ask for user approval before high-impact or destructive actions.
5. Verify write-back results by reading the app state again.

## Common workflows

### Workflow name

1. Read required context.
2. Decide based on user goals and app state.
3. Write structured result back through MCP.
4. Verify the result.
5. Report concise outcome to the user.

## Failure handling

- If MCP is unavailable, check the runbook.
- If app state is inconsistent, stop and ask for review.
- If approval is required, do not write final changes without user confirmation.
