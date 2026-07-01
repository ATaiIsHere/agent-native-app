# AI Logic Contract

An AI Logic Contract declares where an app expects a personal agent to perform AI reasoning.

It answers:

- What event triggers AI work?
- What context should the agent read?
- What output should the agent produce?
- Which tool writes the result back?
- Is user approval required?
- How can the behavior be evaluated?

## Example

```yaml
ai_tasks:
  generate_plan:
    description: Generate a goal plan from user state and goal definition.
    trigger: goal.created
    read_context:
      - goal
      - user_state
      - recent_task_history
    output_schema: PlanDraft
    writeback_tool: mq_generate_plan
    approval: optional
    evals:
      - creates_7_to_14_day_cycle
      - includes_measurable_milestones
      - avoids_overloading_user

  review_reward_pricing:
    description: Review whether a reward conflicts with active goals.
    trigger: reward.created
    read_context:
      - reward
      - active_goals
    output_schema: RewardImpactReview
    writeback_tool: mq_review_reward_pricing
    approval: sometimes
```

## Approval levels

Suggested convention:

```yaml
approval:
  none: safe autonomous action
  optional: agent may proceed, but should explain changes
  required: ask user before write-back
  review_required: write proposal only; human must approve before commit
```

## Permission levels

Suggested convention:

```yaml
permission_level:
  0: read only
  1: write notes or proposals
  2: create/update normal app records
  3: modify app configuration or automation
  4: destructive, deployment, migration, credentials, or bulk data changes
```

Default personal agents should autonomously operate only levels 0-2 unless the user has explicitly granted a higher scope.
