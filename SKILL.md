---
name: gpt56-model-router
description: Recommend and apply an appropriate GPT-5.6 Sol, Terra, or Luna model and reasoning effort for a new project or task. Use when a user starts work and needs an easy explanation of the best model, intelligence level, cost-saving alternative, quality-first alternative, or an execution handoff using the recommended configuration.
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

## Explain the recommendation

Return this short, user-friendly structure:

```text
추천: [모델] + [추론강도]
한 줄 이유: [난이도·실패 비용·속도·반복량을 연결한 설명]
절약안: [더 낮은 조합] — [언제 안전한지]
품질 우선안: [더 높은 조합] — [언제 필요한지]
적용: [자동 위임 가능 여부 또는 사용자가 선택할 위치]
검증: 대표 작업 5~20개로 품질, 재작업률, 시간, 비용을 비교
```

Do not expose an internal scoring formula unless the user asks. Never present a cost or quality guarantee. State assumptions briefly when information is missing.

## Apply the recommendation safely

Separate **recommendation** from **application**.

1. Recommend first. Apply a changed configuration only when the user asks to start, proceed, or apply it.
2. Check the current session's available agent/model controls before claiming an automatic change succeeded.
3. When the selected model and effort are available to an agent-spawn tool, delegate the actual project work to one worker with exactly those settings. Give it the user task, acceptance criteria, and the compact routing rationale. Continue as coordinator and report that the work was delegated with that configuration.
4. When the environment cannot set the selected configuration programmatically, say so plainly. Give the exact model and effort to select in the composer or project configuration; do not claim it was changed.
5. For **ultra / multi-agent**, only create parallel workers after identifying independent workstreams. Otherwise use Sol max or xhigh as appropriate.
6. Do not change global or project defaults without the user's explicit request. Do not create an API key, alter billing, or change account settings.

In Codex, an unpinned agent may choose a configuration automatically. For reproducibility, pin `model` and `model_reasoning_effort` when the available agent configuration supports the recommendation. If Luna is recommended but unavailable to the current programmatic agent controls, keep the recommendation and instruct the user to select Luna manually rather than silently substituting Sol or Terra.

## Calibrate with results

After a representative run, adjust one dimension at a time:

- Quality, correctness, or rework failure: increase effort first, then escalate Terra to Sol.
- Excess delay or cost with acceptable quality: reduce effort first, then move Sol to Terra or Terra to Luna.
- A decomposable project that is slow: consider multi-agent work; do not use it for a single tightly coupled question.

Keep the user's chosen quality, latency, and budget priorities above the default rules.
