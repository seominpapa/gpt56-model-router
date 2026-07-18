# Agent Guidance

When responding to a project model-routing request, give a user-readable recommendation with the model, reasoning effort, and a concrete reason tied to complexity, failure cost, speed, scale, and tool use.

Always include bounded estimates with their assumptions:

- elapsed time to the stated delivery boundary;
- aggregate input/output token range, including verification and rework; and
- cost range only with verified current official rates or rates supplied by the user; otherwise give the pricing formula and mark the currency estimate as needing rates.

After the user approves execution, the root must always delegate the project to one pinned project orchestrator using the recommended model and effort. The root only routes, reports status, and handles user decisions. The orchestrator performs tightly coupled work directly; if it proposes workers, first confirm that workstreams are independent. It remains responsible for integration and final validation. State each worker role, its model and reasoning effort, the reason for the split, and the plan-level elapsed-time, total-token, and cost ranges. Compare it with an orchestrator-only baseline, since parallel work can shorten the critical path but usually raises aggregate tokens and cost.

Do not include time-saving or token-saving alternatives in the initial recommendation. Present a model/effort adjustment only after a qualified repeated-error or rework signal, or when the user explicitly requests a change. Include its reason and predicted quality, elapsed-time, total-token, and cost impact. Do not recommend changes that violate stated quality, risk, privacy, or deadline requirements.

Use ranges and label uncertainty; never promise time, tokens, cost, or quality. Read `references/estimation.md` for the complete estimation method.

After presenting the initial recommendation, require one adjustment-policy choice: `fixed`, `confirm once`, or `automatic`. Do not ask further policy questions. Retain the initial recommendation as the baseline.

Evaluate only a user request or a qualified signal: the same acceptance test fails twice, the same error returns after a fix, the same acceptance criterion needs two rework cycles, or a subagent fails acceptance. Ignore one-off failures and external-tool outages. Compare acceptance/test results, concrete errors, rework, elapsed time, available usage data, and the remaining work's complexity, failure cost, and independence. If token data is unavailable, label the result as an estimate.

Before changing settings, report the old and proposed model/reasoning effort, evidence and reason, predicted quality/time/token/cost impact, and whether it applies to the next work unit, a replacement project orchestrator, or a new worker. Apply `fixed` as report-only; ask exactly once for `confirm once`, then honor its approval or rejection for the rest of the project; apply justified settings automatically for `automatic`. Adjust one dimension at a time: effort before model tier; use parallel workers only for independent bottlenecks; wait for validation before another adjustment. Never change the running root agent. Read `references/adjustment-workflow.md` for the complete workflow.
