# Harness — 에이전트 팀 아키텍처 팩토리 (revfactory)

> Sources: revfactory (GitHub revfactory/harness), 2026-07-17
> Raw: [2026-07-17-revfactory-harness](../../raw/ai-agents/2026-07-17-revfactory-harness.md)

## Overview

revfactory의 **Harness**는 도메인 설명 한 줄을 **에이전트 팀과 그들이 쓸 스킬로
자동 변환**하는 Claude Code 플러그인이다. "하네스 구성해줘"(또는 영어·일본어)라고
말하면, 여섯 개의 사전 정의된 **팀 아키텍처 패턴** 중 하나를 골라 `.claude/agents/`의
에이전트 정의와 `.claude/skills/`의 스킬을 생성한다. 스스로를 "팀을 만드는 메타 스킬",
"L3 메타 팩토리"로 규정한다. 라이선스 Apache 2.0.

> 이름 주의: 이 저장소는 [Harness 100](harness-100.md)과 이름이 겹치지만 **다른
> 프로젝트**다. Harness 100은 완성된 팀 100개를 모아둔 **템플릿 컬렉션**이고,
> revfactory Harness는 도메인에 맞는 팀을 **그 자리에서 설계·생성하는 팩토리**다.
> (컬렉션 vs 팩토리)

## L3 메타 팩토리 — 팀 아키텍처를 찍어내는 층

Harness는 자신을 Claude Code 생태계의 **L3 Meta-Factory 층**, 그중 "팀 아키텍처
팩토리" 하위층으로 위치시킨다. 이웃 도구와의 구분이 정체성을 드러낸다.

- **Archon (L3 런타임 구성 팩토리)**: 결정적(deterministic) 런타임 구성을 생성.
- **meta-harness (L3 Codex 런타임 포트)**: 같은 기능의 Codex 환경판.
- **ECC (L2 Cross-Harness Workflow)**: 여러 하네스에 걸쳐 스킬·규칙을 표준화.

핵심 대비: **Archon은 "런타임 구성"을 찍어내고, Harness는 "팀 아키텍처(+ 그 팀이 쓸
스킬)"를 찍어낸다.** 즉 무엇을 실행할지가 아니라, **누가 어떤 구조로 협업할지를**
생성한다.

## 6가지 팀 아키텍처 패턴

도메인 성격에 따라 아래 중 하나를 골라 팀을 짠다.

| 패턴 | 언제 쓰나 |
|---|---|
| **Pipeline** | 순차 의존 작업 (앞 단계 산출이 다음 단계 입력) |
| **Fan-out/Fan-in** | 병렬 독립 작업 후 결과 통합 |
| **Expert Pool** | 맥락에 따라 필요한 전문가만 선택 호출 |
| **Producer-Reviewer** | 생성→검토 사이클로 품질 보증 |
| **Supervisor** | 중앙 에이전트가 작업을 동적으로 분배 |
| **Hierarchical Delegation** | 위에서 아래로 재귀적 과제 분해 |

이 패턴들은 이 브레인이 이미 수집한 [클로드 에이전트 스택](claude-agent-stack.md)의
"병렬 다중 에이전트 토론"(Fan-out), "가상 개발 조직"(Supervisor/Hierarchical)과
같은 구조를 **명시적 카탈로그로 정리한** 것이다.

## 6단계 생성 워크플로우

도메인 분석 → 팀 아키텍처 설계 → 에이전트 정의 생성 → 스킬 생성 →
통합·오케스트레이션 → 검증·테스트.

산출물은 `.claude/` 아래에 정리된다 — 에이전트 정의(`analyst.md`·`builder.md`·`qa.md` 등)와,
컨텍스트 효율을 위한 **Progressive Disclosure(점진적 공개)** 방식의 스킬 파일.

## 설치·호출

```
/plugin marketplace add revfactory/harness
/plugin install harness@harness-marketplace
```

트리거: "Build a harness for this project" / "하네스 구성해줘" 등. 실험 기능이라
`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 환경변수가 필요하다.

## 효과 주장 (검증 주의)

저자 자체 측정 A/B 연구(n=15, 소프트웨어 엔지니어링 과제)에서 품질 점수 49.5 → 79.3
(**+60%**), 15/15 승률, 출력 분산 −32%를 보고했다. **다만 저자 스스로 "제3자 재현은
아직"이라고 밝힌 자체 측정치**이므로, 이 저장소의 [벤더 수치 주장 경계] 원칙에 따라
방향성으로만 받아들인다.

## 왜 지금 중요한가 — "체계를 갖춘 개발"

즉흥적 바이브 코딩이 아니라 **역할·구조·검증 루프를 갖춘 팀**으로 개발하려는 흐름의
도구다. [Matt Pocock 스킬 모음](mattpocock-skills.md)이 개인의 스킬 규율을 다룬다면,
Harness는 그 위에서 **팀 구조 자체를 설계**한다. [Harness 100](harness-100.md)의
완성 템플릿과 함께 쓰면, "패턴 선택 → 팀 생성 → 스킬 규율 적용"의 체계가 완성된다.

## See Also

- [Harness 100 (에이전트 팀 하네스 컬렉션)](harness-100.md) — 완성된 팀 100개 템플릿(컬렉션)
- [클로드 에이전트 스택 (자율형 AI 팀 5가지 세팅)](claude-agent-stack.md) — 팀 세팅 골격
- [Skills For Real Engineers (Matt Pocock)](mattpocock-skills.md) — 팀이 쓸 스킬의 규율
