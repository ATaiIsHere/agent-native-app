# Challenges and Objections

This repository describes an emerging pattern, not a finished standard. These are likely objections and the current working answers.

## 1. Is this just MCP?

No.

MCP answers:

```text
What tools and resources can an agent call?
```

Agent-native App Architecture also asks:

```text
Which app workflows need AI reasoning?
When are they triggered?
What state should the agent read?
What schema should the agent produce?
Which tool writes the result back?
What requires user approval?
How is the app deployed, backed up, and verified?
```

MCP is a key protocol layer. It is not the whole app packaging pattern.

## 2. Is this just OpenAI Apps SDK or MCP Apps?

No.

MCP Apps and Apps SDK focus on interactive UI inside an AI host.

Agent-native App Architecture is broader: it includes runtime, database, human UI, MCP/API, Skill, AI logic contract, operations, permissions, and app-owned state.

## 3. Is the name already used?

Yes, similar language exists in the community.

This repository explores a specific branch of the broader Agent-native idea:

> apps that delegate AI logic to the user's personal agent, treating the personal agent as the external AI runtime.

It does not claim to have invented the phrase.

## 4. Why not embed AI inside each app?

Embedding AI can be useful for product-specific features.

But long-term memory, cross-app context, personal preferences, and high-level decisions are often better handled by the user's personal agent.

A sleep app, task app, calendar, and health dashboard may each know one slice of the user's life. The personal agent can reason across all of them.

## 5. Who is this for today?

Early adopters:

- personal agent builders
- self-hosted software users
- local-first app developers
- internal tool builders
- developer-tool ecosystems
- people building agent-operable workflows

This is not yet a mass-market consumer pattern.

## 6. What are the security risks?

The risks are real:

- unauthenticated tool access
- excessive permissions
- prompt injection through tool results
- tool poisoning
- malicious or compromised Skills
- token exposure
- missing audit trails
- destructive write-back by mistake

This pattern only works if permissions, approval, auditing, and least privilege are first-class concepts.

## 7. How should trust be handled?

Possible controls:

- read-only tools by default
- permission levels in the app manifest
- approval rules in the AI logic contract
- scoped tokens
- audit logs for agent actions
- signed releases or provenance metadata
- static scanning for Skills and MCP servers
- human approval for destructive or high-impact changes

## 8. Isn't the AI Logic Contract too abstract?

It can be, if overdesigned.

The minimum useful contract should stay small:

```yaml
trigger: event or condition
read_context: state the agent should inspect
output_schema: structured result expected
writeback_tool: tool used to save the result
permission_level: blast radius
approval: none | optional | required | review_required
```

The contract should be proven by working examples, not just YAML.

## 9. Why would SaaS vendors support this?

Many won't, at least initially.

The near-term fit is stronger for:

- open-source apps
- self-hosted apps
- internal tools
- developer tools
- personal data systems
- local-first workflows

Large SaaS products may prefer embedded AI for business reasons. Agent-native patterns are more aligned with user-owned agents and portable app ecosystems.

## 10. What prevents the agent from corrupting app data?

The app still owns validation and invariants.

Agent-native does not mean “trust the agent blindly.”

The app should enforce:

- schema validation
- permission checks
- state transitions
- idempotency
- audit logs
- rollback paths
- human approval gates for high-impact actions

The app's deterministic core should become stronger, not weaker.
