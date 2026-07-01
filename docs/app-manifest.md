# App Manifest

`app.yaml` is a machine-readable overview of an Agent-native App.

It should let a personal agent answer:

- What is this app?
- How do I run it?
- How do I connect to it?
- What capabilities does it expose?
- Where is the skill?
- Where is the AI logic contract?
- How do I verify health?
- How do I back it up?

## Draft schema

```yaml
name: example-app
version: 0.1.0
description: Short description of the app.

runtime:
  type: docker-compose
  compose_file: compose.yaml
  services:
    api:
      url: http://localhost:3001
      healthcheck: http://localhost:3001/health
    ui:
      url: http://localhost:5173
    mcp:
      transport: stdio
      command: docker compose exec -T app node dist/index.js mcp

data:
  database:
    type: sqlite
    path: ./data/app.db
  backup:
    command: ./scripts/backup.sh
    restore_command: ./scripts/restore.sh

agent:
  skill: ./SKILL.md
  ai_logic_contract: ./ai-logic-contract.yaml
  mcp_server_name: example_app

capabilities:
  - name: read_items
    permission_level: 0
  - name: update_items
    permission_level: 2
  - name: migrate_database
    permission_level: 4

operations:
  runbook: ./RUNBOOK.md
  install: docker compose up -d --build
  update: git pull && docker compose up -d --build
  verify: curl -f http://localhost:3001/health
```

This is intentionally small. The manifest should point to deeper documents rather than duplicating everything.
