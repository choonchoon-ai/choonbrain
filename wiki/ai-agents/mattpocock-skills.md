# Skills For Real Engineers (Matt Pocock의 엔지니어링 스킬 모음)

> Sources: Matt Pocock (GitHub mattpocock/skills), 2026-07-17
> Raw: [2026-07-17-mattpocock-skills](../../raw/ai-agents/2026-07-17-mattpocock-skills.md)

## Overview

TypeScript 교육자 Matt Pocock이 **매일 실제 엔지니어링에 쓰는 에이전트 스킬**을 공개한
모음이다. 태그라인 "not vibe coding"이 방향을 요약한다 — 즉흥적으로 코드를 뽑아내는
바이브 코딩이 아니라, **정렬·검증·설계 규율을 갖춘 진짜 엔지니어링**을 에이전트에게
시키기 위한 워크플로우 스킬들이다. Claude Code 등에서 슬래시 명령/스킬로 설치해 쓴다.
라이선스 MIT.

## 4대 실패 모드와 대응 스킬

이 모음의 설계 철학은 AI 지원 개발에서 반복되는 **4가지 실패 모드**를 각각의 스킬로
막는 것이다.

| 실패 모드 | 문제 | 대응 스킬 |
|---|---|---|
| **Misalignment(오정렬)** | 에이전트가 의도와 다른 것을 만든다 | `/grill-me`, `/grill-with-docs` — 착수 전 상세 인터뷰 |
| **Verbosity(장황함)** | 도메인 용어가 없어 소통이 길어진다 | `CONTEXT.md`·ADR로 공유 언어 구축 → 토큰·소통 비용 절감 |
| **Broken Code(깨진 코드)** | 피드백 루프 부족 | `/tdd`, `/diagnosing-bugs` — 일관된 검증 루프 |
| **Poor Architecture(설계 퇴화)** | 코드베이스가 유지보수 불가로 썩는다 | `/improve-codebase-architecture`, `/to-spec` — 설계 규율 유지 |

특히 **오정렬을 착수 전에 잡는다**는 점이 핵심이다 — 이는 이 브레인이 이미 수집한
[인터뷰 기법("나를 심문하라")](claude-code-ai-employees.md)과 정확히 같은 원리다.
`grill`(캐묻다)이라는 이름 자체가 "결과물을 시키기 전에 AI가 먼저 질문하게 한다"는
발상을 담는다.

## 스킬 분류 — User-invoked vs Model-invoked

이 모음은 스킬을 **누가 호출하는가**로 나눈다. 이 구분은 이 저장소의
[스킬 vs 플러그인](claude-code-ai-employees.md) 정리를 더 정교하게 만든다.

- **User-invoked(사용자가 슬래시 명령으로 호출)**
  - 엔지니어링: `ask-matt`, `grill-with-docs`, `triage`,
    `improve-codebase-architecture`, `setup-matt-pocock-skills`, `to-spec`,
    `to-tickets`, `implement`, `wayfinder`
  - 생산성: `grill-me`, `handoff`, `teach`, `writing-great-skills`
- **Model-invoked(모델이 판단해 자동 호출)**
  - 엔지니어링: `prototype`, `diagnosing-bugs`, `research`, `tdd`,
    `domain-modeling`, `codebase-design`, `code-review`, `resolving-merge-conflicts`
  - 생산성: `grilling`

`writing-great-skills`처럼 **스킬을 잘 쓰는 법 자체를 스킬로** 담은 메타 스킬,
`handoff`(작업 인계)처럼 세션·사람 간 연속성을 돕는 스킬이 눈에 띈다.

## CONTEXT.md — 공유 언어를 문서로 박제한다

Verbosity 대응책인 `CONTEXT.md`는 프로젝트의 도메인 용어·규칙을 문서로 고정해 에이전트와
사람이 같은 언어로 대화하게 만든다. 이는 이 브레인의 **`SKILL.md`/`CLAUDE.md` 운영
매뉴얼**과 같은 역할이며, [claw.md 운영 매뉴얼](../knowledge-management/llm-wiki-build-guide.md)·
[메모리·컨텍스트 시스템](../ai-tools/ai-model-tool-reviews.md)과 같은 계열이다. 즉
"맥락을 대화가 아니라 파일에 둔다"는 이 저장소의 일관된 원칙과 맞닿는다.

## 설치

- **skills.sh(편집·복사 가능)**: `npx skills@latest add mattpocock/skills`
- **Claude Code 플러그인(읽기 전용·자동 업데이트)**:
  `claude plugin marketplace add mattpocock/skills` →
  `claude plugin install mattpocock-skills@mattpocock`
- 이후 저장소마다 한 번 `/setup-matt-pocock-skills` 실행.

## See Also

- [클로드 코드로 AI 직원 만들기 (AIMAX 팁 모음)](claude-code-ai-employees.md) — 인터뷰 기법·스킬 vs 플러그인·learnings.md
- [Hermes Agent (Nous Research의 자기개선형 AI 에이전트)](hermes-agent.md) — 스킬을 경험에서 자동 생성·개선하는 학습 루프
- [AI 모델·도구 리뷰 (AIMAX 채널)](../ai-tools/ai-model-tool-reviews.md) — 스킬(Skills) 패턴과 메모리·컨텍스트 시스템
- [Harness — 에이전트 팀 아키텍처 팩토리 (revfactory)](revfactory-harness.md) — 개인 스킬 규율 위에서 팀 구조 자체를 설계하는 상위 레이어
