# Mochi Quest as an Agent-native App

Mochi Quest is a personal growth / quest system used as a conceptual example of Agent-native App Architecture.

## Deterministic app core

The app owns:

- goals
- tasks
- rewards
- streaks
- wallet / coins
- assessments
- task completion ledger
- active plan state
- API validation
- UI rendering

## Agent-delegated AI logic

The personal agent owns:

- goal clarification
- plan generation
- replan decisions
- task difficulty adjustment
- reward conflict review
- daily review
- user-facing coaching tone
- cross-app context interpretation

## Why this is agent-native

Mochi Quest does not need to embed a separate chatbot with its own memory.

Instead, it exposes tools and state. The user's personal agent reads that state, applies long-term user context, makes AI decisions, and writes structured results back into the app.
