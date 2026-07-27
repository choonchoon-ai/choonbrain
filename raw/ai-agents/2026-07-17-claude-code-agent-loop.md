# How the agent loop works (Claude Code / Agent SDK 공식 문서)

> Source: https://code.claude.com/docs/en/agent-sdk/agent-loop
> Collected: 2026-07-17
> Published: Unknown

---

<!-- 접근 가능한 공식 문서에서 캡처한 발췌. 원문을 충실히 보존한다.
     긴 코드 예제는 포맷 노이즈로 보고 요지만 남긴다. -->

The Agent SDK lets you embed Claude Code's autonomous agent loop in your own
applications. When you start an agent, the SDK runs the same execution loop that
powers Claude Code: Claude evaluates your prompt, calls tools to take action,
receives the results, and repeats until the task is complete.

## The loop at a glance

1. **Receive prompt.** Claude receives your prompt, system prompt, tool definitions,
   and conversation history. SDK yields a SystemMessage subtype "init".
2. **Evaluate and respond.** Claude may respond with text, request tool calls, or both
   (AssistantMessage).
3. **Execute tools.** SDK runs each requested tool, collects results, feeds them back.
   Hooks can intercept/modify/block tool calls.
4. **Repeat.** Steps 2–3 repeat; each full cycle is one turn. Continues until Claude
   produces a response with no tool calls.
5. **Return result.** Final AssistantMessage (no tool calls) + ResultMessage (final
   text, token usage, cost, session ID).

## Turns and budget

- A turn = one round trip: Claude outputs tool calls, SDK executes, results feed back.
  Turns continue until Claude produces output with no tool calls → loop ends.
- `max_turns` / `maxTurns`: cap tool-use round trips (default: no limit).
- `max_budget_usd` / `maxBudgetUsd`: cap cost before stopping (default: no limit).
  Budget covers subagents; once spent reaches cap, spawning another subagent fails with
  "Budget limit reached". (v2.1.217+)
- On limit hit: ResultMessage with subtype `error_max_turns` or `error_max_budget_usd`.
- "Setting a budget is a good default for production agents."

## Effort level

`effort` controls reasoning depth per turn: low(파일 조회) · medium(일상 편집) ·
high(리팩터·디버깅) · xhigh(코딩·에이전트 작업; Fable 5·Opus 4.7+·Sonnet 5 권장) ·
max(다단계 심층). Extended thinking과는 독립적.

## Permission mode

default · acceptEdits(파일 편집·mkdir/touch/mv/cp 자동승인) · plan(편집 없이 탐색·계획) ·
dontAsk · auto(모델 분류기가 승인/거부) · bypassPermissions(격리 환경 전용, root 불가).

## The context window

세션 내 턴 사이에 리셋되지 않고 누적된다(시스템 프롬프트·툴 정의·대화 이력·툴 입출력).
변하지 않는 부분(시스템 프롬프트·툴 정의·CLAUDE.md)은 자동 prompt cache된다.

### Automatic compaction
컨텍스트가 한계에 가까워지면 오래된 이력을 요약해 공간 확보(compact_boundary 메시지).
지속 규칙은 초기 프롬프트가 아니라 CLAUDE.md에 둔다(매 요청 재주입되므로). PreCompact 훅,
`/compact` 수동 트리거 가능.

### Keep context efficient
- 서브에이전트로 서브태스크 분리(각자 새 대화로 시작, 최종 응답만 부모로 반환).
- 툴을 최소 집합으로 스코프.
- MCP tool search로 스키마 지연 로드.
- 단순 작업엔 낮은 effort.

## Sessions and continuity

각 상호작용은 세션을 생성/이어간다. ResultMessage.session_id로 나중에 resume.
resume 시 이전 턴의 전체 맥락(읽은 파일·분석·수행 동작) 복원. fork로 분기 가능.
스테이트리스 컨테이너/서버리스에서는 session_store 어댑터로 트랜스크립트를 자기 백엔드에 미러.

## Handle the result

ResultMessage.subtype: success · error_max_turns · error_max_budget_usd ·
error_during_execution · error_max_structured_output_retries. `result`(최종 텍스트)는
success에서만. 모든 subtype이 total_cost_usd·usage·num_turns·session_id를 가진다.
stop_reason: end_turn · max_tokens · refusal.

## Hooks

PreToolUse(툴 실행 전 검증·차단) · PostToolUse(감사·부수효과) · UserPromptSubmit(맥락 주입) ·
Stop(결과 검증·세션 저장) · SubagentStart/Stop(병렬 결과 집계) · PreCompact(요약 전 아카이브).
훅은 앱 프로세스에서 실행되어 컨텍스트를 소비하지 않으며, 루프를 단락(short-circuit)할 수 있다.

## 문서가 가리키는 실전 가이드(블로그)

문서 말미: "For a practical guide to designing loops in Claude Code, from turn-based to
goal-based and proactive loops, see 'Loop engineering: getting started with loops' on
the blog." → https://claude.com/blog/getting-started-with-loops

---

## [별첨] 사용자가 공유한 블로그 (본문 미접근)

> https://claude.com/blog/getting-started-with-loops
> 제목: "Loop engineering: Getting started with loops" (Claude by Anthropic)
> 발행: 2026-06-30 (검색 기준)
>
> ⚠️ claude.com/blog는 이 클라우드 세션의 egress 정책상 차단되어 **본문을 가져오지 못했다.**
> 아래는 검색 메타데이터와 위 공식 문서의 참조 문장으로 **확인된 뼈대만** 기록한다.
> 블로그 전문은 사용자가 붙여넣어 주면 보강한다.
>
> - 다룬다고 확인된 루프 유형: turn-based(턴 기반), goal-based(목표 기반), proactive(선제적).
>   (검색 요약은 여기에 time/schedule을 더해 "4가지"로 소개)
> - 관련 명령/스킬(검색 기준): `/goal`, `/loop`, `/schedule`, verification 스킬.
> - 정지 조건(stop condition)까지 실행되는 루프 설계가 핵심 주제.
