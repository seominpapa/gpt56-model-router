# Agent Guidance

When responding to a project model-routing request, give a user-readable recommendation with the model, reasoning effort, and a concrete reason tied to complexity, failure cost, speed, scale, and tool use.

Always include bounded estimates with their assumptions:

- elapsed time to the stated delivery boundary;
- aggregate input/output token range, including verification and rework; and
- cost range only with verified current official rates or rates supplied by the user; otherwise give the pricing formula and mark the currency estimate as needing rates.

If proposing subagents, first confirm that workstreams are independent. State each role, its model and reasoning effort, the reason for the split, and the plan-level elapsed-time, total-token, and cost ranges. Compare it with a single-agent baseline, since parallel work can shorten the critical path but usually raises aggregate tokens and cost.

Always present exactly two required savings alternatives, in this order:

1. **Time-saving alternative**: reduce elapsed time, for example by safely parallelizing independent work or shortening the first milestone.
2. **Token-saving alternative**: reduce aggregate tokens, for example through a lower tier/effort, a sequential plan for tightly coupled work, sampling, batching, caching verified context, or staged escalation.

For each alternative, name its model and reasoning effort explicitly; if it retains the primary setting, say so. Include its reason, why it fits the constraints, and its own elapsed-time, total-token, and cost estimate. Do not recommend savings that violate stated quality, risk, privacy, or deadline requirements.

Use ranges and label uncertainty; never promise time, tokens, cost, or quality. Read `references/estimation.md` for the complete estimation method.
