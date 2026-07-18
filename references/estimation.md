# Estimation Rules

Use these rules whenever the routing response includes time, token, or cost expectations.

## Set the estimate boundary

Define the result being estimated: a first draft, reviewable implementation, tested release candidate, or finished delivery. State the expected inputs, tool/test duration, review rounds, and whether the work can run independently in parallel.

Even when these details are incomplete, produce a provisional orchestrator and worker/no-worker plan using stated minimal assumptions. Ask clarifying questions after, not instead of, the initial plan.

## Produce ranges

Use low–likely–high reasoning internally, then report a practical range. Include aggregate input and output tokens for the project orchestrator, retries, verification, and synthesis; add every worker only when the orchestrator delegates. If outer root-conversation overhead is material, label it separately. Do not imply that model output, external-tool latency, or human review time is guaranteed.

For a parallel plan, report both:

- **Elapsed time**: the critical path, including coordination and final synthesis.
- **Total tokens/cost**: the orchestrator-only total for a direct plan, or the orchestrator, workers, and coordination for a delegated plan; the latter can exceed the direct plan.

Always compare a delegated plan with the orchestrator-only baseline. The root conversation is a routing and approval layer, not a substitute for the project orchestrator.

## Calculate cost safely

Use a current official model price only if it can be verified at the time of the answer. Otherwise ask for the team's input/output token rates or provide this formula without a numeric currency value:

`estimated cost = input tokens × input rate + output tokens × output rate + tool or provider charges`

Keep currency and billing units explicit. If the user asks for a rough budget before rates are known, give token ranges and label cost as "단가 필요".

## Compare a requested adjustment

Do not offer savings alternatives with the initial recommendation. When a qualified repeated-error/rework signal occurs or the user requests a change, state the mechanism, why it fits the constraints, and its estimated quality, elapsed-time, total-token, and cost impact. Good levers are adjusting effort before tier, using one agent for tightly coupled work, splitting only independent work, or escalating after a quality gate.

Do not hide tradeoffs: lower effort can increase rework; parallel agents can shorten elapsed time while raising aggregate tokens and cost.

## Compare the baseline

Preserve the initial estimate as a baseline. When the user requests a settings adjustment or a qualified repeated-error/rework signal occurs, compare observed elapsed time and available usage with that baseline. Separate measured data from estimates. Show the expected delta for quality, elapsed time, total tokens, and cost after the proposed change.

Change effort before model tier unless observed evidence shows a capability mismatch. Evaluate parallel workers separately: they may reduce elapsed time while increasing aggregate tokens and cost. Apply the selected policy: fixed, confirm-once, or automatic. Do not use elapsed time alone as a trigger.
