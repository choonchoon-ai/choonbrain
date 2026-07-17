# Skills For Real Engineers (Matt Pocock)

> Source: https://github.com/mattpocock/skills
> Collected: 2026-07-17
> Published: Unknown

---

<!-- GitHub README에서 캡처한 발췌. 원문을 충실히 보존한다. -->

# Skills For Real Engineers

태그라인: "Skills for Real Engineers. Straight from my .agents directory."
소개: "My agent skills that I use every day to do real engineering - not vibe coding."

Primary language: Shell (77.2%). Author: Matt Pocock. License: MIT.

## The Four Failure Modes & Solutions (AI 지원 개발의 4대 실패 모드)

1. **Misalignment** — 에이전트가 의도와 다른 것을 만든다.
   - 해결: `/grill-me`, `/grill-with-docs` 스킬이 작업 시작 전 상세 인터뷰를 진행.
2. **Verbosity** — 에이전트가 도메인 전용 용어를 몰라 장황해진다.
   - 해결: `CONTEXT.md` 문서와 ADR로 공유 언어를 만들어 소통 비용·토큰 사용을 줄인다.
3. **Broken Code** — 피드백 루프 부족.
   - 해결: `/tdd`(테스트 주도 개발), `/diagnosing-bugs` 스킬로 일관된 검증.
4. **Poor Architecture** — 코드베이스가 유지보수 불가로 퇴화.
   - 해결: `/improve-codebase-architecture`, `/to-spec` 스킬로 설계 규율 유지.

## Installation

**Via skills.sh (editable, copyable):**
```bash
npx skills@latest add mattpocock/skills
```

**Via Claude Code Plugin (read-only, auto-updating):**
```bash
claude plugin marketplace add mattpocock/skills
claude plugin install mattpocock-skills@mattpocock
```

이후 저장소마다 한 번 `/setup-matt-pocock-skills` 실행.

## Skill Categories

**Engineering Skills (User-invoked):** ask-matt, grill-with-docs, triage,
improve-codebase-architecture, setup-matt-pocock-skills, to-spec, to-tickets,
implement, wayfinder

**Engineering Skills (Model-invoked):** prototype, diagnosing-bugs, research, tdd,
domain-modeling, codebase-design, code-review, resolving-merge-conflicts

**Productivity Skills (User-invoked):** grill-me, handoff, teach, writing-great-skills

**Productivity Skills (Model-invoked):** grilling
