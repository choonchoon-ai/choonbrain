# 루프 엔지니어링 (Claude Code 에이전트 루프 설계)

> Sources: Claude Code Agent SDK 공식 문서 "How the agent loop works", 2026-07-17; Anthropic 블로그 "Loop engineering: Getting started with loops", 2026-06-30 (본문 미접근)
> Raw: [2026-07-17-claude-code-agent-loop](../../raw/ai-agents/2026-07-17-claude-code-agent-loop.md)

## Overview

**루프 엔지니어링(Loop Engineering)**은 Claude를 한 번의 응답이 아니라 **정지 조건에
도달할 때까지 도구를 호출하며 반복(loop)하는 에이전트**로 설계하는 접근이다. 즉흥적인
단발 프롬프트 대신, "무엇을 언제 멈출지"를 설계해 자율 작업을 굴린다. 이 글은 사용자가
공유한 Anthropic 블로그 "Loop engineering"(실전 4가지 루프)과, 그 기반 메커니즘을
설명하는 **공식 Claude Code Agent SDK 문서**를 함께 정리한다.

> ⚠️ **출처 주의**: 블로그 본문(claude.com/blog)은 이 클라우드 세션에서 접근이 차단돼
> **직접 읽지 못했다.** 아래 "실전 4가지 루프"는 검색 메타데이터와 공식 문서의 참조
> 문장으로 **확인된 뼈대만** 담았고, 세부는 블로그 전문을 붙여넣으면 보강한다. 반면
> "루프의 작동 원리" 절은 접근 가능한 공식 문서에서 그대로 가져온 내용이다.

## 루프의 작동 원리 (공식 문서 기준)

에이전트 세션은 항상 같은 주기를 돈다:

1. **프롬프트 수신** → 2. **평가·응답**(텍스트 또는 도구 호출) → 3. **도구 실행**(결과를
다시 Claude에게 피드백) → 4. **반복**(2–3 = 한 **턴**; 도구 호출 없는 응답이 나올 때까지) →
5. **결과 반환**(최종 텍스트 + 비용·토큰·세션 ID).

즉 **"도구 호출이 없어지는 순간"이 기본 정지 조건**이다. 이 자연 종료 위에 명시적
제어장치를 얹는다.

### 정지 조건과 예산 — 폭주를 막는 장치

- **`max_turns`**: 도구 사용 턴 수 상한 (기본: 무제한).
- **`max_budget_usd`**: 멈추기 전 비용 상한 (기본: 무제한). **서브에이전트 지출도 포함**되어,
  한도 도달 시 새 서브에이전트 생성이 "Budget limit reached"로 실패한다.
- 한도에 걸리면 결과가 `error_max_turns` / `error_max_budget_usd`로 돌아온다.
- 문서 권고: **"프로덕션 에이전트엔 예산 설정이 좋은 기본값"** — 열린 프롬프트("이 코드베이스
  개선해줘")는 무제한이면 길게 폭주하기 때문.

### effort(추론 강도)와 permission mode

- **effort**: low→max로 턴당 추론 깊이를 조절(단순 조회는 low로 비용·지연 절감,
  리팩터·디버깅은 high 이상). Fable 5·Opus 4.7+·Sonnet 5에는 xhigh 권장.
- **permission mode**: default · acceptEdits · plan · dontAsk · auto · bypassPermissions로
  루프가 도구를 얼마나 자율 실행할지 통제. 자율 루프일수록 격리 환경 + 명시적 허용규칙이 필수.

### 컨텍스트·연속성 — 루프가 길어질 때

- 컨텍스트는 턴 사이에 **누적**된다. 한계에 가까워지면 **자동 압축(compaction)**으로 오래된
  이력을 요약한다. **지속 규칙은 프롬프트가 아니라 CLAUDE.md에 둔다**(매 요청 재주입·prompt
  cache되므로). — 이는 이 브레인의 [위키 기반 연속성](../knowledge-management/wiki-continuity.md)
  원칙과 정확히 같다: 맥락을 대화가 아니라 파일에 둔다.
- **서브에이전트로 서브태스크 분리**: 각자 새 대화로 시작하고 최종 응답만 부모로 반환 →
  메인 컨텍스트를 얇게 유지. (병렬화는 [Harness 6패턴](revfactory-harness.md)의 Fan-out과 연결)
- **세션 resume/fork**: `session_id`로 이전 맥락을 복원하거나 분기.

### 훅(Hooks) — 루프 중간에 개입

PreToolUse(실행 전 검증·차단)·PostToolUse·UserPromptSubmit·Stop(결과 검증)·
SubagentStart/Stop·PreCompact. 훅은 앱 프로세스에서 돌아 컨텍스트를 소비하지 않고, 위험한
도구 호출을 **단락(short-circuit)**할 수 있다. → [Matt Pocock 스킬](mattpocock-skills.md)의
`/tdd`·검증 루프를 훅으로 강제하는 것과 같은 발상.

## 실전 4가지 루프 (블로그 — 확인된 뼈대)

공식 문서는 말미에 이 블로그를 "turn-based, goal-based, proactive 루프 설계 실전
가이드"로 명시한다. 검색 요약은 여기에 time/schedule을 더해 **4가지**로 소개한다.

| 루프 | 개념(확인된 수준) | 관련 명령 |
|---|---|---|
| **턴 기반(turn-based)** | 정해진 턴/스텝 단위로 반복 | — |
| **목표 기반(goal-based)** | 목표(정지 조건) 달성까지 자율 반복 | `/goal` |
| **시간 기반(time/schedule)** | 일정에 맞춰 주기 실행 | `/schedule` |
| **선제적(proactive)** | 조건 감지 시 스스로 작업 시작 | — |
| (공통 반복 실행) | 프롬프트/명령을 주기 반복 | `/loop` |

> 참고: 이 세션에도 실제 `/loop` 스킬이 있다 — "프롬프트나 슬래시 명령을 주기적으로
> 반복 실행(예: `/loop 5m /foo`)". `/loop`·`/goal`·`/schedule`은 이 taxonomy의 실물 구현이다.
> 각 루프의 세부 설계·예시는 **블로그 전문을 붙여넣으면 이 표를 확정·확장**한다.

## 이 브레인과의 연결 — "체계를 갖춘 개발"의 심장

루프 엔지니어링은 지금까지 수집한 조각들을 하나로 꿰는 개념이다. [커리어해커
알렉스](../ai-native-careers/careerhackeralex.md)의 "루프 엔지니어링" 실무, [Fable
루프](../ai-tools/ai-model-tool-reviews.md)(목표+루브릭+검증자로 자기교정),
[Hermes의 학습 루프](hermes-agent.md), [Harness의 팀 패턴](revfactory-harness.md)이 모두
**"정지 조건까지 자율 반복 + 검증"**이라는 같은 골격을 공유한다. 사용자의 "이런 식으로
체계를 갖춰 개발하고 싶다"는 목표가 바로 이 루프 설계에 해당한다.

## See Also

- [커리어해커 알렉스](../ai-native-careers/careerhackeralex.md) — 루프 엔지니어링 실무 관점
- [AI 모델·도구 리뷰 (Fable 루프)](../ai-tools/ai-model-tool-reviews.md) — 목표·루브릭·검증자 자기교정
- [Hermes Agent](hermes-agent.md) · [Harness 팀 팩토리](revfactory-harness.md) · [Matt Pocock 스킬](mattpocock-skills.md)
