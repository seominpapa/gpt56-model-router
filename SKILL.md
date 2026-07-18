---
name: gpt56-model-router
description: Recommend and apply an appropriate GPT-5.6 Sol, Terra, or Luna model and reasoning effort for a new project or task. Use when a user starts work and needs an easy explanation of the best model, reasoning level, rationale, project time/token/cost estimate, cost-saving alternatives, quality-first alternative, or a Codex subagent handoff with a generated pinned agent configuration.
---

# GPT-5.6 Model Router

Help users start projects with enough capability, without paying for more reasoning than the work needs. Explain choices in plain Korean when the user writes Korean; otherwise mirror the user's language.

## Gather only the missing facts

If the task description already answers a question, do not ask it again. Before recommending, collect these six facts in one compact message:

1. What outcome should the project produce?
2. How difficult is it: simple, normal, complex, or a hard problem?
3. What happens if the result is wrong: low, medium, high, or critical impact?
4. Is speed important: immediate, seconds, or minutes are acceptable?
5. Is this a one-off, repeatable work, or high-volume processing?
6. Does it need tools, coding, or independently parallelizable subprojects?

Treat security or personal-data sensitivity as a handling constraint, not by itself as a reason to select a stronger model. Ask for the intended handling boundary if it is material and missing.

If the user says “recommend for me” without answers, use **Terra + medium** as the provisional choice and label it provisional. Do not block a simple start on a long interview.

## Choose the model tier first

Read [decision-rules.md](references/decision-rules.md) when a borderline case, scoring rationale, or exact task mapping is needed.

- Choose **Luna** for simple, low-risk, high-volume or latency-sensitive work: classification, routing, extraction, translation, short summaries, or customer-support drafts.
- Choose **Terra** as the normal default for documents, reports, standard RAG answers, meeting notes, ordinary automation, and most project work.
- Choose **Sol** for complex reasoning, engineering agents, debugging, architecture, strategy, broad research, difficult math/science, or decisions with high failure cost.
- Escalate Terra to Sol when quality failure needs substantial rework, a human is about to make a consequential decision, or tool-based verification is central to success.
- Do not choose Sol merely to make an ordinary draft sound better. Do not choose Luna for complex synthesis or high-stakes validation.

## Choose reasoning effort after the model

- **low**: straightforward, bounded tasks where speed matters.
- **medium**: balanced default for most work.
- **high**: multi-step analysis, strategy comparisons, complex research, or nontrivial debugging.
- **xhigh**: architecture review, difficult debugging, high-risk analysis, or advanced technical validation.
- **max**: a genuinely hard problem or critical final review where extra verification is worth the delay and cost.
- **ultra / multi-agent**: only when the task can be split into independent pieces and shorter elapsed time matters. Treat it as a parallel-workflow choice, not a universal effort setting.

Avoid Luna at xhigh/max/ultra. Treat Terra at max or ultra as a signal to compare Sol instead. Prefer testing Terra high before Sol when the task is important but not clearly high-stakes.

## Estimate before explaining

Read [estimation.md](references/estimation.md) before presenting project estimates or alternatives. Use ranges, not a single promised number.

State the assumptions that materially affect the estimate: deliverables, amount/quality of input context, tool or test time, review cycles, and whether workstreams are independent. Estimate all of the following:

- **Elapsed time**: likely wall-clock range from start to a reviewable result.
- **Total tokens**: aggregate input and output token range across the root and every worker.
- **Cost**: a range only when the user gives a rate or a current official rate can be verified. Otherwise show the token estimate and the pricing formula; never invent a price.

For a subagent plan, show the single-agent baseline and the proposed worker roles. Explain that parallelism can reduce elapsed time while increasing aggregate tokens and cost. Include each role's model and effort, its reason, and the plan-level elapsed-time, total-token, and cost range.

## Explain the recommendation

Return this short, user-friendly structure:

```text
추천: [모델] + [추론강도]
한 줄 이유: [난이도·실패 비용·속도·반복량을 연결한 설명]
예상: [가정] · 완료 시간 [범위] · 총 토큰 [범위] · 비용 [범위 또는 산식]
하위 에이전트: [불필요 / 역할·각 모델+추론강도·추천 이유] · 시간·토큰·비용 영향
절약안 1: [대안] — [추천 이유] · 시간 [범위] · 토큰 [범위] · 비용 [범위 또는 산식]
절약안 2: [대안] — [추천 이유] · 시간 [범위] · 토큰 [범위] · 비용 [범위 또는 산식]
품질 우선안: [더 높은 조합] — [언제 필요한지]
적용: [자동 위임 가능 여부 또는 사용자가 선택할 위치]
검증: 대표 작업 5~20개로 품질, 재작업률, 시간, 비용을 비교
```

Recommend at least two viable time- or token-saving alternatives. Consider a lower effort/tier, a single-agent sequential plan instead of parallel workers, narrower first-deliverable scope, representative sampling, batching, caching/reusing verified context, or staged escalation. Do not recommend an alternative that breaks the stated quality, risk, privacy, or deadline constraint. Give each alternative's reason and estimated time, tokens, and cost in the same form as the primary recommendation.

Do not expose an internal scoring formula unless the user asks. Never present a cost, token, time, or quality guarantee. State assumptions briefly when information is missing.

## Offer a pinned Codex subagent

After every non-provisional recommendation, offer this next step in plain language: **“권장 설정으로 하위 에이전트를 만들어 바로 시작할까요?”** If the user says start, proceed, apply, or gives equivalent approval, create the pinned project agent automatically.

Do not change the current root conversation's model. Create a new subagent so its model and reasoning settings are explicit and reproducible.

## Create and run the pinned agent

1. Check that the current Codex surface supports the recommended model and effort. Do not substitute another model silently.
2. Create `.codex/agents/` in the current project if needed. Use the personal directory `~/.codex/agents/` only when the user explicitly asks for a cross-project default.
3. Write `.codex/agents/gpt56-router-<tier>-<effort>.toml`, replacing `<tier>` and `<effort>` with the recommendation. Use the current model ID and effort in this template:

```toml
# Generated by gpt56-model-router for this project.
name = "gpt56-router-<tier>-<effort>"
description = "Pinned <tier> / <effort> worker generated for this project."
developer_instructions = "Execute only the delegated project task. Follow its acceptance criteria. Report completed work, validation, and unresolved risks."
model = "gpt-5.6-<tier>"
model_reasoning_effort = "<effort>"
```

4. If that file already exists and carries the generated comment, update it to the new recommendation. Never overwrite a non-generated agent file; create a distinct router filename instead and explain why.
5. Spawn the agent by its custom-agent name when the current agent-creation surface supports named custom agents. Otherwise spawn with the same `model` and `model_reasoning_effort` passed directly to the available spawn control.
6. Give the worker the user task, acceptance criteria, relevant project context, and the routing rationale. Report the config path and the exact pinned settings before work begins.
7. For **ultra / multi-agent**, first identify independent workstreams. Create one pinned agent file per role only when the available surface supports the required effort; otherwise state that the setting cannot be applied automatically and offer the closest supported single-agent plan.

Create or modify these configuration files only after the user asks to start, proceed, apply, or explicitly requests configuration changes. Do not change global defaults, billing, API keys, or account settings.

If the recommended Luna, max, or ultra setting is unavailable in the current agent controls, preserve the recommendation and ask the user to select it manually. Do not claim that a configuration file or a worker changed the setting unless the creation surface confirms it.

## Calibrate with results

After a representative run, adjust one dimension at a time:

- Quality, correctness, or rework failure: increase effort first, then escalate Terra to Sol.
- Excess delay or cost with acceptable quality: reduce effort first, then move Sol to Terra or Terra to Luna.
- A decomposable project that is slow: consider multi-agent work; do not use it for a single tightly coupled question.

Keep the user's chosen quality, latency, and budget priorities above the default rules.
