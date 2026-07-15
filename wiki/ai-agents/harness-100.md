# Harness 100 (에이전트 팀 하네스 컬렉션)

> Sources: revfactory/harness-100 (GitHub README_ko), 2026-07-14
> Raw: [harness-100-agent-team-collection](../../raw/ai-agents/harness-100-agent-team-collection.md)

## Overview

Harness 100은 **Claude Code의 에이전트 팀(agent team) 기능으로 도메인 전문가 4~5명이 협업하는 프로덕션급 워크플로우 100개**를 모아 둔 오픈소스 컬렉션이다(Apache 2.0). 콘텐츠 제작부터 개발·데이터·비즈니스·법률·교육·운영까지 10개 카테고리를 아우르며, 각 하네스는 그대로 복사해 자신의 프로젝트에 바로 붙여 쓸 수 있는 재사용 가능한 "팀 템플릿"이다. 전체 규모는 하네스 100개, 에이전트 정의 489개, 오케스트레이터 스킬 100개, 총 689개 파일이다.

## 핵심 개념: 하네스란

하나의 하네스 = **하나의 목표를 위해 협업하는 에이전트 팀 + 이를 지휘하는 오케스트레이터 스킬**의 묶음이다. 개별 에이전트에게 일을 시키는 대신, 여러 전문가 에이전트가 `SendMessage`로 직접 통신하고 서로의 산출물을 교차 검증하며 파이프라인을 완성한다.

## 폴더 구조

모든 하네스는 동일한 구조를 따라 이식성이 높다:

```
{NN}-{harness-name}/
└── .claude/
    ├── CLAUDE.md            # 프로젝트 개요·사용법
    ├── agents/              # 전문 에이전트 정의 4~5개
    │   └── {agent}.md
    └── skills/
        └── {skill-name}/skill.md   # 오케스트레이터 스킬
```

적용은 단순 복사다: `cp -r 01-youtube-production/.claude/ /path/to/my-project/.claude/`.

## 품질 기준 (모든 하네스 공통)

- **에이전트 팀 모드**: SendMessage 직접 통신 + 교차 검증
- **도메인 전문성**: 분야별 실전 프레임워크·방법론 내장
- **산출물 템플릿**: 에이전트별 구조화된 출력 포맷
- **의존 관계 관리**: 작업 순서와 병렬 실행 명시
- **에러 핸들링**: 실패 시 폴백 전략
- **작업 규모별 모드**: 풀/축소/단일 모드 지원
- **테스트 시나리오**: 정상·기존파일활용·에러 3종 흐름
- **트리거 경계**: should-trigger + NOT-trigger 명시

## 10개 카테고리 구성

| 카테고리 | 범위(번호) | 예시 하네스 |
|---|---|---|
| 1. 콘텐츠 제작 & 크리에이티브 | 01~15 | youtube-production, podcast-studio, newsletter-engine |
| 2. 소프트웨어 개발 & DevOps | 16~30 | fullstack-webapp, code-reviewer, security-audit |
| 3. 데이터 & AI/ML | 31~42 | ml-experiment, llm-app-builder(프롬프트→RAG→평가), bi-dashboard |
| 4. 비즈니스 & 전략 | 43~55 | startup-launcher, market-research, financial-modeler |
| 5. 교육 & 학습 | 56~65 | language-tutor, thesis-advisor, knowledge-base-builder |
| 6. 법률 & 규정 | 66~72 | contract-analyzer, privacy-engineer, patent-drafter |
| 7. 건강 & 라이프스타일 | 73~80 | meal-planner, travel-planner, personal-finance |
| 8. 커뮤니케이션 & 문서 | 81~88 | technical-writer, proposal-writer, crisis-communication |
| 9. 운영 & 프로세스 | 89~95 | hiring-pipeline, onboarding-system, feedback-analyzer |
| 10. 전문 도메인 | 96~100 | real-estate-analyst, ecommerce-launcher, ip-portfolio |

## 도메인별 내장 프레임워크

하네스가 단순 프롬프트가 아니라 "전문가 팀"인 이유는 각 도메인의 실전 프레임워크를 내장하기 때문이다. 예: 개발(SOLID·DDD·OWASP Top 10·DORA), 비즈니스(BMC·TAM/SAM/SOM·Porter's 5 Forces·RICE·OKR), 교육(Bloom's Taxonomy·ADDIE·CEFR), 문서(Diataxis·PREP·STAR·MADR·SemVer), 데이터(Star/Snowflake 스키마·SHAP/LIME) 등.

## 이 브레인과의 연결점

- 이 두 번째 브레인 자체가 "수집→마크다운 위키→검색 인덱스" 구조인데, Harness 100의 **#64 knowledge-base-builder** 하네스가 동일한 지식관리 파이프라인을 에이전트 팀으로 구현한 사례다.
- **#41 llm-app-builder**(프롬프트→RAG→평가→최적화)는 [LLM Wiki](../knowledge-management/llm-wiki.md)가 다루는 "RAG 대신 사전 합성" 논의와 대비해 볼 만하다.

## See Also

- [클로드 에이전트 스택 (자율형 AI 팀 5가지 세팅)](claude-agent-stack.md) — 이런 에이전트 팀을 세팅하는 5단계 골격
- [LLM Wiki (LLM이 유지하는 지식 위키)](../knowledge-management/llm-wiki.md)
