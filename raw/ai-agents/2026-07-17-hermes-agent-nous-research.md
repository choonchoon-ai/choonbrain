# Hermes Agent (Nous Research)

> Source: https://github.com/nousresearch/hermes-agent
> Collected: 2026-07-17
> Published: Unknown

---

<!-- GitHub README에서 캡처한 발췌. 원문 마크다운을 충실히 보존한다.
     별점/포크 수치는 요약 과정의 신뢰도가 낮아 원문 확정치가 아니므로 여기 기록하지 않는다. -->

# Hermes Agent ☤

**The self-improving AI agent built by Nous Research.** It's the only agent with a built-in learning loop — it creates skills from experience, improves them during use, nudges itself to persist knowledge, searches its own past conversations, and builds a deepening model of who you are across sessions.

레포 태그라인(별도 표기): "The agent that grows with you"

## Quick Install

### Linux, macOS, WSL2, Termux

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

### Windows (native, PowerShell)

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

After installation:

```bash
source ~/.bashrc    # reload shell (or: source ~/.zshrc)
hermes              # start chatting!
```

## Key Features

| Feature | Description |
|---------|-------------|
| **A real terminal interface** | Full TUI with multiline editing, slash-command autocomplete, conversation history, interrupt-and-redirect, and streaming tool output. |
| **Lives where you do** | Telegram, Discord, Slack, WhatsApp, Signal, and CLI — all from a single gateway process. |
| **A closed learning loop** | Agent-curated memory with periodic nudges. Autonomous skill creation after complex tasks. Skills self-improve during use. |
| **Scheduled automations** | Built-in cron scheduler with delivery to any platform. |
| **Delegates and parallelizes** | Spawn isolated subagents for parallel workstreams. |
| **Runs anywhere** | Six terminal backends — local, Docker, SSH, Singularity, Modal, and Daytona. |
| **Research-ready** | Batch trajectory generation, trajectory compression for training tool-calling models. |

## 추가로 확인된 사항 (페이지·요약 기준)

- Primary language: Python (82.0%)
- 요구사항: Python 3.11+, Node.js. Windows 네이티브 설치기는 Python 3.11·Node.js·ripgrep·ffmpeg·MinGit 동봉.
- 메모리: FTS5 전문 검색으로 세션 히스토리 검색 + LLM 요약으로 세션 간 회상. Honcho dialectic 프레임워크 호환 사용자 모델링.
- 도구: 코드 실행·웹 브라우징·파일·미디어 등 40+ 통합 도구, MCP(Model Context Protocol) 서버 지원.
- Skills: agentskills.io 호환, 경험에서 스킬 자동 생성·사용 중 자기개선.
- 서버리스 지속성: Modal·Daytona에서 유휴 시 하이버네이트.
- 라이선스: MIT.
