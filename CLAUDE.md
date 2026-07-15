# 프로젝트 지침

이 저장소는 개인 지식 위키(두 번째 브레인)다. 이 저장소에서 작업할 때는
반드시 먼저 `SKILL.md`를 읽고 그 규칙(Ingest / Query / Lint)을 따른다.

핵심 요약:

- 자료 수집 요청 → SKILL.md의 Ingest 절차 (raw/ 보존 → wiki/ 컴파일 → 연쇄 갱신 → index/log 갱신)
- 지식 질문 → SKILL.md의 Query 절차 (wiki/index.md에서 출발, 위키 내용 우선, 출처 링크 인용)
- 점검 요청 → SKILL.md의 Lint 절차
- raw/의 기존 파일은 절대 수정하지 않는다
- 위키 본문은 한국어, 파일·폴더명은 영문 kebab-case
- 템플릿은 references/ 폴더의 것을 그대로 사용한다
- **지식 그래프**: Ingest 때 `wiki/graph/graph.json`에 노드·관계를 추가한다 (SKILL.md "지식 그래프" 참고)
- **웹 발행**: 이 위키는 GitHub Pages로 발행 중이다. Ingest / Archive / Lint 자동수정 등 wiki를 바꾸는 작업 뒤에는 사이트를 재배포한다 (SKILL.md "웹 발행" 참고)

## 어느 기기에서든 (데스크톱 CLI · Claude Code 웹/모바일)

이 저장소는 폰(claude.ai/code 또는 Claude 모바일 앱의 Code 탭)에서도 조작한다.
클라우드 세션은 로컬 설정·메모리를 못 보므로, 모든 운영 규칙은 이 저장소 안(SKILL.md·CLAUDE.md)에 있다.
그러니 **어떤 세션이든** Ingest/Archive/Lint 후 SKILL.md "웹 발행" 절차로 사이트를 재배포해 웹과 저장소를 항상 일치시킨다.
