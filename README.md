# Agent-native App Architecture

> **Personal Agent as the AI Runtime for Apps.**

Agent-native App Architecture is an emerging application pattern: apps expose deterministic state, tools, events, and UI, while delegating AI decision-making to the user's **personal agent** instead of embedding isolated assistants inside every app.

## Working definition

An **Agent-native App** is an application designed to be understood, operated, and extended by a user's personal agent.

The app owns the reliable system parts:

- data model and persistence
- deterministic business rules
- API and human UI
- permissions and validation
- MCP tools/resources
- events and webhooks
- deployment and operations contract

The user's personal agent owns the AI parts:

- intent understanding
- planning and recommendation
- summarization and analysis
- cross-app reasoning
- long-term user context
- human-in-the-loop decisions
- writing structured results back to the app

## What this is not

Agent-native App Architecture is **not**:

- an app with an embedded chatbot
- a SaaS-specific assistant per product
- a replacement for MCP
- a new LLM runtime
- a new agent framework
- a requirement that every app must ship its own agent

It is a packaging and integration pattern for making apps usable by personal agents.

## Why it matters

Most AI apps today create islands:

```text
App A + its own chatbot + isolated memory
App B + its own chatbot + isolated memory
App C + its own chatbot + isolated memory
```

The user has to repeat context. Memory is fragmented. Permissions are scattered. Each app only sees a slice of the user's life.

Agent-native Apps invert the relationship:

```text
Personal Agent
  ├─ user memory
  ├─ reasoning / planning
  ├─ cross-app orchestration
  └─ approval policy
        ↓
Agent-native Apps
  ├─ deterministic core
  ├─ UI / API / DB
  ├─ MCP tools
  ├─ events
  └─ AI logic contracts
```

The personal agent becomes the external AI runtime. Apps remain reliable deterministic systems.

## Traditional AI App vs Agent-native App

```text
Traditional AI App:
App + embedded AI + isolated memory + product-specific chatbot

Agent-native App:
App + deterministic core + tools/events/contracts
Personal Agent = external AI runtime + user memory + cross-app reasoning
```

## Architecture layers

1. **Runtime Layer**
   - Docker / Compose
   - API server
   - database
   - web UI
   - background workers

2. **Deterministic Core**
   - state machine
   - validation
   - permissions
   - business invariants
   - migrations

3. **Agent Connector Layer**
   - MCP tools
   - MCP resources/prompts when useful
   - events/webhooks
   - structured schemas

4. **AI Logic Contract**
   - AI decision points
   - trigger conditions
   - required context
   - expected output schema
   - write-back tools
   - approval rules
   - eval cases

5. **Agent Guidance Layer**
   - `SKILL.md`
   - operating policy
   - examples
   - failure handling
   - human approval rules

6. **Ops Contract**
   - `README.md`
   - `RUNBOOK.md`
   - healthcheck
   - backup / restore
   - migration / rollback
   - troubleshooting

## Minimal app package

A minimal Agent-native App should provide:

```text
app.yaml                     # machine-readable app manifest
ai-logic-contract.yaml       # AI delegation points
SKILL.md                     # how a personal agent should use the app
README.md                    # human + agent setup overview
RUNBOOK.md                   # operations, backup, restore, troubleshooting
compose.yaml                 # reproducible runtime
MCP server                   # tools/resources exposed to agents
Human UI                     # direct user control and visibility
Healthcheck                  # verifiable runtime status
```

## Example: Mochi Quest

Mochi Quest is a personal growth / quest system.

The app owns deterministic logic:

- goals, tasks, rewards, streaks, wallet, assessments
- task completion and coin ledger
- plan records and active plan state
- API validation
- web UI rendering

The personal agent owns AI logic:

- generating plans
- adjusting difficulty
- interpreting skipped tasks
- reviewing reward conflicts
- deciding when to notify or ask the user
- using long-term user context across apps

See [`examples/mochi-quest/`](examples/mochi-quest/) for a conceptual package.

## Current status

This repository is a concept draft, not a finished standard.

The goal is to collect language, examples, templates, and feedback around a practical pattern that combines:

- Docker / Compose for runtime reproducibility
- MCP for agent-to-app tools and data
- Agent Skills for operational knowledge and behavior guidance
- app manifests and AI logic contracts for app-level delegation

## Discussion questions

- Is “Agent-native App” the right name?
- What belongs in the app manifest?
- How should apps declare AI decision points?
- How should permissions and human approval be represented?
- How much of this should be MCP metadata vs app-level convention?
- What makes an app safe for a personal agent to operate?

## License

MIT. See [`LICENSE`](LICENSE).
