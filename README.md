# GPT-5.6 Model Router

프로젝트를 시작할 때 필요한 질문을 짧게 묻고, GPT-5.6 **Sol**, **Terra**, **Luna**와 적절한 추론 강도(`low`–`max`, `ultra`)를 추천하는 Agent Skill입니다. 비용 절약안, 품질 우선안, 그리고 적용 가능한 방법도 함께 제시합니다.

## 무엇을 해 주나요?

- 작업 난이도, 실패 비용, 속도, 반복량, 도구·병렬 작업 필요성을 간단히 파악합니다.
- 일반 업무는 `Terra + medium`을 기본으로 삼고, 저위험 대량 작업은 Luna로 낮추며, 복잡·고위험 작업은 Sol로 올립니다.
- `ultra` 또는 multi-agent는 독립적인 하위 과제로 나뉘는 큰 작업에만 권장합니다.
- 현재 도구가 실제 모델·추론 강도를 바꿀 수 있는지 확인한 뒤에만 자동 적용을 주장합니다.

## 빠른 사용

설치한 뒤 새 대화에서 다음처럼 요청하세요.

```text
이 프로젝트에 가장 적합한 모델과 추론강도를 추천해줘.
프로젝트: 여러 데이터 소스에서 경쟁사 리서치를 수집하고 보고서를 작성한다.
```

정보가 부족하면 스킬이 핵심 질문만 묻습니다. 바로 시작하려면 “추천한 설정으로 진행해줘”라고 말하세요.

## 설치

먼저 저장소를 내려받습니다.

```bash
git clone https://github.com/seominpapa/gpt56-model-router.git
cd gpt56-model-router
```

### Codex

개인 스킬 폴더에 복사하면 모든 프로젝트에서 사용할 수 있습니다.

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R . "${CODEX_HOME:-$HOME/.codex}/skills/gpt56-model-router"
```

Codex를 새로 시작한 뒤 `$gpt56-model-router`를 호출하거나, 모델 선택 요청을 자연어로 입력하세요.

### Claude Code

Claude Code는 Agent Skills의 `SKILL.md` 형식을 지원합니다. 개인 스킬로 설치하려면 다음을 실행하세요.

```bash
mkdir -p ~/.claude/skills
cp -R . ~/.claude/skills/gpt56-model-router
```

특정 프로젝트에서만 쓰려면 해당 프로젝트 루트의 `.claude/skills/` 아래에 복사하세요.

```bash
mkdir -p .claude/skills
cp -R /path/to/gpt56-model-router .claude/skills/gpt56-model-router
```

`/gpt56-model-router`로 호출하거나 프로젝트 시작 시 모델·추론 강도 추천을 요청할 수 있습니다.

### Cursor

프로젝트 루트에 다음 경로로 배치하세요.

```bash
mkdir -p .cursor/skills
cp -R /path/to/gpt56-model-router .cursor/skills/gpt56-model-router
```

Cursor Agent를 다시 열고 `/gpt56-model-router`를 호출하거나, 새 프로젝트의 모델·추론 강도 추천을 요청하세요.

## 플랫폼별 적용 범위

이 스킬의 추천 로직은 세 도구에서 공통으로 사용할 수 있지만, 실제 모델 변경은 도구와 계정의 모델 제공 범위에 따라 다릅니다.

| 도구 | 추천 | GPT-5.6 설정 적용 |
|---|---|---|
| Codex | 지원 | 현재 세션의 모델·추론 설정 또는 에이전트 설정에서 지원될 때 적용 가능 |
| Claude Code | 지원 | Claude는 OpenAI GPT-5.6을 직접 실행하지 않으므로 추천·작업 설계 용도로 사용 |
| Cursor | 지원 | 계정에 해당 모델과 추론 설정이 표시될 때만 수동 또는 에이전트 설정으로 적용 가능 |

스킬은 지원되지 않는 설정을 적용했다고 말하지 않습니다. 대신 선택해야 할 모델과 추론 강도를 명확히 알려줍니다.

## 구성

```text
SKILL.md                       # 공통 라우팅 워크플로
references/decision-rules.md   # 티어·추론강도·승격 규칙
agents/openai.yaml             # Codex UI 메타데이터(다른 도구에서는 무시 가능)
```

## 라이선스

[MIT](LICENSE)
