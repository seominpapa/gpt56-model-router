# Estimation Rules

Use these rules whenever the routing response includes time, token, or cost expectations.

## Set the estimate boundary

Define the result being estimated: a first draft, reviewable implementation, tested release candidate, or finished delivery. State the expected inputs, tool/test duration, review rounds, and whether the work can run independently in parallel.

## Produce ranges

Use low–likely–high reasoning internally, then report a practical range. Include aggregate input and output tokens for every agent, retries, verification, and synthesis. Do not imply that model output, external-tool latency, or human review time is guaranteed.

For a parallel plan, report both:

- **Elapsed time**: the critical path, including coordination and final synthesis.
- **Total tokens/cost**: the sum of all workers plus coordination; this can exceed a single-agent plan.

## Calculate cost safely

Use a current official model price only if it can be verified at the time of the answer. Otherwise ask for the team's input/output token rates or provide this formula without a numeric currency value:

`estimated cost = input tokens × input rate + output tokens × output rate + tool or provider charges`

Keep currency and billing units explicit. If the user asks for a rough budget before rates are known, give token ranges and label cost as "단가 필요".

## Compare alternatives

Offer at least two feasible alternatives. For each, state the mechanism, why it is safe for the stated constraints, and its estimated elapsed time, total tokens, and cost. Good levers are reducing effort before tier, using one agent for tightly coupled work, splitting only independent work, narrowing the first milestone, sampling before full-scale processing, batching, caching verified context, and escalating after a quality gate.

Do not hide tradeoffs: lower effort can increase rework; parallel agents can shorten elapsed time while raising aggregate tokens and cost.
