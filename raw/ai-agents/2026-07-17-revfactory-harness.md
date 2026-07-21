# Harness — Team-Architecture Factory for Claude Code (revfactory)

> Source: https://github.com/revfactory/harness
> Collected: 2026-07-17
> Published: Unknown

---

<!-- GitHub README에서 캡처한 발췌. 원문을 충실히 보존한다. -->

# Harness — The Team-Architecture Factory for Claude Code

태그라인: "A meta-skill that designs domain-specific agent teams, defines specialized
agents, and generates the skills they use."

Opening: "Harness is a team-architecture factory for Claude Code. Say 'build a harness
for this project' (English) or '하네스 구성해줘' (한국어) or 'ハーネスを構成して'
(日本語), and the plugin turns your domain description into an agent team and the skills
they use — picked from six pre-defined team-architecture patterns."

Primary language: HTML (100.0%). Author: revfactory. License: Apache 2.0.

## Architectural Layer Positioning

Harness operates at the **L3 Meta-Factory layer**, specifically the **Team-Architecture
Factory sub-layer**. 이웃 접근들과의 구분:

- **L3 Runtime-Configuration Factory (Archon)**: 결정적 런타임 구성을 생성.
- **L3 Codex Runtime Port (meta-harness)**: Codex 환경용 동등 기능.
- **L2 Cross-Harness Workflow (ECC)**: 여러 하네스에 걸쳐 스킬/규칙을 표준화.

핵심 구분: "Archon generates deterministic runtime configurations. Harness generates
team architectures (pipeline, fan-out/fan-in, expert pool, producer-reviewer,
supervisor, hierarchical delegation) plus the skills agents use."

## Six Team-Architecture Patterns

| Pattern | Purpose |
|---------|---------|
| **Pipeline** | Sequential dependent tasks |
| **Fan-out/Fan-in** | Parallel independent tasks with consolidated results |
| **Expert Pool** | Context-dependent selective agent invocation |
| **Producer-Reviewer** | Quality assurance through generation-then-review cycles |
| **Supervisor** | Central agent with dynamic task distribution |
| **Hierarchical Delegation** | Top-down recursive task decomposition |

## Six-Phase Workflow

Domain Analysis → Team Architecture Design → Agent Definition Generation →
Skill Generation → Integration & Orchestration → Validation & Testing

## Installation & Usage

**Via Marketplace:**
```
/plugin marketplace add revfactory/harness
/plugin install harness@harness-marketplace
```

Trigger phrases: "Build a harness for this project," "Design an agent team for this
domain," or "Set up a harness" in Claude Code (requires
`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`).

## Output Structure

Generated files organize under `.claude/`:
- `.claude/agents/` — individual agent definitions (analyst.md, builder.md, qa.md, etc.)
- `.claude/skills/` — capability files with Progressive Disclosure for context efficiency

## Research Evidence

An A/B study (n=15, author-measured) across software engineering tasks showed:
49.5 → 79.3 average quality score (+60%), 15/15 win rate, −32% output variance.
Authors note third-party replications are pending.
