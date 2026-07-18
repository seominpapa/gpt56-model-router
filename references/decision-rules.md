# GPT-5.6 routing reference

Use this reference only for nuanced routing. It distills the supplied “GPT-5.6 Sol·Terra·Luna 모델 라우터 설계 문서” (2026-07-16) and the current Codex model-selection guidance.

## Tier roles

| Tier | Primary role | Good fits | Avoid when |
|---|---|---|---|
| Luna | speed and cost | routing, classification, short summaries, extraction, support drafts, repetitive automation | task needs complex reasoning or rigorous validation |
| Terra | balanced workhorse | reports, drafting, meeting notes, general RAG, routine automation | the final result carries high consequence or deep technical verification |
| Sol | depth and verification | strategy, coding agents, debugging, architecture, broad research, high-risk analysis | a basic draft or bulk low-risk processing |

## Effort roles

| Effort | Use for |
|---|---|
| low | clear, small, fast tasks |
| medium | normal business and knowledge work; default |
| high | complex analysis, research, strategy, debugging |
| xhigh | difficult technical or high-risk review |
| max | hard problems and critical final validation |
| ultra / multi-agent | independent streams of a large project; reduced wall-clock time may justify extra cost |

## Task starting points

| Task | Start with | Escalate if |
|---|---|---|
| Fast question, translation, short summary | Luna low or Terra low | accuracy requires multi-source synthesis |
| Meeting notes, report draft, routine RAG | Terra medium | material factual or decision risk appears |
| Business plan, strategy comparison, complex research | Terra high | reasoning and validation requirements become substantial; use Sol high |
| Coding agent, debugging, architecture | Sol high or xhigh | repeated failures, difficult edge cases, or final high-impact decision; use Sol max |
| Final decision, hard math/science | Sol max | independent research or implementation streams exist; consider multi-agent |
| Large research or parallel implementation | Sol ultra / multi-agent | only after confirming independent subproblems |

## Escalation and evaluation

Start with the least expensive configuration likely to meet the quality bar. Compare 5–20 representative tasks; record correctness, rework, hallucinations, latency, token cost, and user satisfaction. Escalate for a demonstrated quality miss; downshift when quality is stable and cost or speed matters.

## Application boundaries

A skill is an instruction workflow, not an unconditional control plane. It can tell an available Codex agent-spawn mechanism to run a worker with a pinned model and effort, but it must check that those controls exist. It must never claim a model switch that the current product surface did not confirm.
