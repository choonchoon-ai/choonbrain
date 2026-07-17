# Hermes Agent (Nous Research의 자기개선형 AI 에이전트)

> Sources: Nous Research (GitHub nousresearch/hermes-agent), 2026-07-17
> Raw: [2026-07-17-hermes-agent-nous-research](../../raw/ai-agents/2026-07-17-hermes-agent-nous-research.md)

## Overview

Hermes Agent는 Nous Research가 만든 **자기개선형(self-improving) AI 에이전트**다.
스스로 "내장 학습 루프(built-in learning loop)를 가진 유일한 에이전트"라고 소개하며,
핵심은 **경험에서 스킬을 만들고, 사용 중 그 스킬을 개선하고, 지식을 스스로 저장하도록
자기 자신을 유도(nudge)하며, 과거 대화를 검색하고, 세션을 넘어 사용자 모델을
점점 깊게 쌓는** 순환 구조에 있다. 태그라인은 "The agent that grows with you".
CLI로 시작해 여러 메시징 플랫폼까지 하나의 게이트웨이로 연결한다. 라이선스는 MIT.

## 닫힌 학습 루프 (Closed Learning Loop)

이 프로젝트의 정체성은 학습 루프다. 세 요소로 구성된다.

1. **경험 기반 스킬 자동 생성** — 복잡한 작업을 마친 뒤 절차를 스킬로 승격한다.
   (agentskills.io 호환)
2. **사용 중 자기개선** — 만들어진 스킬을 쓰면서 스스로 다듬는다.
3. **에이전트 큐레이션 메모리 + 주기적 넛지** — 지식을 저장하도록 주기적으로 자신을
   유도해 기억을 영속화한다.

기억은 **FTS5 전문 검색**으로 세션 히스토리를 뒤지고, LLM 요약으로 세션 간 회상을
지원한다. 사용자 모델링은 Honcho dialectic 프레임워크와 호환된다.

## 아키텍처·기능

- **터미널 UI**: 멀티라인 편집, 슬래시 명령 자동완성, 대화 히스토리, 중단-후-방향전환,
  스트리밍 도구 출력.
- **어디서나 산다(게이트웨이)**: Telegram·Discord·Slack·WhatsApp·Signal·CLI를 단일
  게이트웨이 프로세스로 통합. (음성 메모 전사 포함)
- **40+ 통합 도구 + MCP**: 코드 실행·웹 브라우징·파일·미디어 등. Model Context
  Protocol 서버를 지원한다.
- **서브에이전트**: 격리된 서브에이전트를 띄워 작업을 병렬화.
- **스케줄 자동화**: 내장 cron 스케줄러가 어느 플랫폼으로든 결과를 전달.
- **6개 터미널 백엔드**: local·Docker·SSH·Singularity·Modal·Daytona. Modal·Daytona는
  서버리스로 유휴 시 하이버네이트한다.
- **연구용**: 도구호출 모델 학습을 위한 배치 트라젝토리 생성·압축.
- **모델 자유**: Nous Portal·OpenRouter·OpenAI·커스텀 엔드포인트를 코드 수정 없이 교체.
- **요구사항**: Python 3.11+, Node.js. (Windows 네이티브 설치기는 의존성 동봉)

## 이 브레인과의 연결 — 학습 루프는 같은 뿌리다

Hermes의 "경험 → 스킬 → 자기개선" 루프는 이 저장소가 이미 수집한 두 개념과 정확히
같은 계열이다.

- [클로드 코드로 AI 직원 만들기](claude-code-ai-employees.md)의 **learnings.md 자가진화
  스킬** — 스킬 옆 파일에 교정을 누적해 '제도적 기억'을 만드는 방식 — 이 Hermes의
  "사용 중 스킬 자기개선"과 대응한다.
- Hermes의 **에이전트 큐레이션 메모리·세션 간 회상**은 지식이 대화가 아니라 저장소에
  쌓여 세션을 넘어 이어진다는 [위키 기반 연속성](../knowledge-management/wiki-continuity.md)과
  같은 문제의식이다. 다만 Hermes는 메모리를 **에이전트가 자동 관리**하고, 이 브레인은
  **Ingest/Lint 규칙으로 명시적으로 관리**한다는 차이가 있다.
- **서브에이전트 병렬화**는 [클로드 에이전트 스택](claude-agent-stack.md)의 병렬 다중
  에이전트, [Harness 100](harness-100.md)의 에이전트 팀 오케스트레이션과 같은 축이다.

## 주의: 미검증 수치

수집 시 페이지 요약에서 별점 216k·포크 40.5k 같은 수치가 관측됐으나, GitHub 최상위급
이상치라 요약 과정의 오류일 가능성이 높다. 이 저장소의 [벤더 수치 주장 경계] 원칙에 따라
**수치는 사실로 채택하지 않는다.** 필요하면 저장소 페이지에서 직접 재확인할 것.

## See Also

- [클로드 코드로 AI 직원 만들기 (AIMAX 팁 모음)](claude-code-ai-employees.md)
- [클로드 에이전트 스택 (자율형 AI 팀 5가지 세팅)](claude-agent-stack.md)
- [위키 기반 연속성 (세션·기기 독립성)](../knowledge-management/wiki-continuity.md)
