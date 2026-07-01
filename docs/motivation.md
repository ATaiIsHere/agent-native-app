# Motivation

This pattern did not start as a framework idea. It came from running a personal agent environment with multiple self-hosted services.

The practical problem was not “how do we add AI to an app?”

It was:

> How do we keep the agent environment clean while letting apps use the user's personal agent for AI logic?

## 1. Keep the agent home clean

Early personal-agent setups often start as a pile of scripts, databases, generated files, caches, and runtime data inside the agent's home directory.

That works briefly, then becomes hard to maintain:

```text
agent-home/
  memories/
  skills/
  config.yaml
  service-db.sqlite
  uploads/
  app-cache/
  generated-runtime-files/
  random-state/
```

The boundary is wrong. The agent home should contain agent context, configuration, and reusable skills — not every app's runtime state.

The fix is to package services as independent Dockerized apps:

```text
agent-home/
  skills/
  config.yaml
  memories/

services/
  app-a/
    compose.yaml
    data/
  app-b/
    compose.yaml
    data/
```

The app owns its runtime. The agent installs only the connector/guidance it needs.

## 2. Some apps need AI logic, but not their own agent lifecycle

Some applications only become useful when AI reasoning is involved.

For example, a personal quest system may need AI to decide:

- how to break down a goal
- whether tasks are too hard
- whether skipped tasks indicate a bad plan
- whether a reward conflicts with another goal
- when to notify the user and when to stay quiet

One option is to embed a separate agent inside each app. But that creates duplicated lifecycle, memory, permissions, prompts, and governance:

```text
quest-agent
office-agent
health-dashboard-agent
file-browser-agent
```

That is not a clean system.

A better boundary is:

```text
App: state, UI, API, validation, MCP tools, events
Personal Agent: user context, reasoning, planning, AI decisions
```

The app exposes APIs/MCP tools. The user's personal agent executes the AI logic and writes structured results back.

## 3. SaaS + Skill is useful, but not always enough

Another option is to use an existing SaaS product and write a Skill telling the agent how to use it.

That works when the SaaS already matches the desired workflow. It breaks down when:

- the Skill becomes tightly coupled to one SaaS data model
- the SaaS cannot represent the workflow you want
- the API/UI changes outside your control
- the app's data cannot be extended cleanly
- a “generic” Skill becomes too abstract to be useful

For personal systems, local-first tools, internal tools, and custom workflows, building a small app can be cleaner than forcing a SaaS to fit.

## The middle path

Agent-native Apps are the middle path:

- the app owns UI, data, APIs, deterministic rules, and runtime state
- the personal agent owns user context, reasoning, planning, and AI decisions
- the agent environment stays clean, installing only Skills and connectors
- app data remains inside each service boundary
- the app does not depend on one specific agent implementation
- AI logic is delegated through explicit APIs, MCP tools, and contracts

This keeps both sides clean:

```text
Agent home stays small.
Apps remain portable.
AI logic remains personal and user-governed.
```
