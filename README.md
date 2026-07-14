# 🧠 Second Brain — 나의 두 번째 브레인

[Astro-Han/karpathy-llm-wiki](https://github.com/Astro-Han/karpathy-llm-wiki)의
LLM Wiki 개념(원안: Andrej Karpathy)을 기반으로 만든 개인 지식 저장소입니다.
**LLM이 위키를 쓰고 유지보수하며, 나는 읽고 질문합니다.**

## 어떻게 다른가

RAG는 질문할 때마다 원문 조각을 검색해서 답을 합성합니다. LLM Wiki는 반대로
**자료가 들어올 때** 지식을 위키 페이지로 합성해 둡니다. 새 자료가 도착하면
기존 글의 요약·상호 참조·인덱스가 갱신되어 지식이 시간이 지날수록 쌓이고 복리화됩니다.

## 구조

```
raw/          → 불변 원본 자료 (아티클, 유튜브 트랜스크립트, 논문, 내 메모)
wiki/         → LLM이 컴파일·유지하는 한국어 지식 페이지
  ├── index.md → 전체 목차
  └── log.md   → 작업 로그
references/   → 템플릿 4종 (raw / article / index / archive)
SKILL.md      → 운영 규칙 전체 (Claude가 이 파일을 따라 작업)
CLAUDE.md     → Claude Code / Cowork 진입점
```

## 사용법

Claude(Cowork, Claude Code 등)에서 이 저장소를 열거나 연결한 뒤:

- **수집(Ingest)** — "이 글 브레인에 넣어줘: <URL>", "오늘 배운 것 기록해줘: ...", PDF 업로드 후 "이 논문 정리해줘"
- **질의(Query)** — "X에 대해 내가 뭘 알고 있지?", "내 위키 기준으로 A와 B 비교해줘"
- **점검(Lint)** — "위키 점검해줘" (깨진 링크·인덱스 불일치는 자동 수정, 내용 모순은 보고)

자세한 규칙은 [SKILL.md](SKILL.md) 참고.

## 원칙

1. raw/는 불변입니다 — 원본은 절대 고쳐 쓰지 않습니다.
2. 위키 본문은 한국어, 파일·폴더명은 영문 kebab-case입니다.
3. 출처 없는 지식은 없습니다 — 모든 글은 Sources/Raw로 원본까지 추적됩니다.
4. 모든 작업은 wiki/log.md에 기록됩니다.
