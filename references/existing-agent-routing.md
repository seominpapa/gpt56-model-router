# Existing Project Agent Routing

Use this reference when the current project root contains `AGENTS.md`, `.codex/agents/*.toml`, or both.

## Inspect and map roles

Read only the current project files. Treat a role as explicit when it has a name and responsibility in `AGENTS.md`, or a `name`/description in an agent TOML. Use matching names and responsibilities to map a role to a TOML file. If more than one file can match, or no match is credible, mark it `매핑 확인 필요`; do not choose a file.

Always retain the explicit role structure. Add a proposed project orchestrator only when none is defined. Do not reinterpret a general instruction in `AGENTS.md` as a new worker role.

## Required recommendation table

Before asking questions, show one row per role:

| 역할 | 출처 | 현재 설정 | 권장 설정 | 이유 | 입력/출력 토큰 | API 제작비용(USD) |
|---|---|---|---|---|---|---|

Use `미설정` for missing model/effort. Sum the role costs into the aggregate recommendation and retain the mandatory orchestrator and worker summary lines.

## Approval and edits

Recommendations are read-only. Apply a change only after the user explicitly approves named roles or all proposed roles.

- For an approved mapped TOML, edit only `model` and `model_reasoning_effort`.
- Preserve `name`, `description`, instructions, permissions, and all other settings.
- For an approved role without a TOML, create `.codex/agents/<role>-router.toml` with the generated-router comment and the role's approved model and effort.
- Do not modify `AGENTS.md`; it remains the role and project-guidance document.
- Do not modify global `~/.codex/agents/` settings without a separate explicit request.
- If mapping is ambiguous, show the candidates and wait for the user to select one.
