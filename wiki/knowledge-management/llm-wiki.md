# LLM Wiki (LLM이 유지하는 지식 위키)

> Sources: Astro-Han (karpathy-llm-wiki), Unknown
> Raw: [2026-07-14-karpathy-llm-wiki-readme.md](../../raw/knowledge-management/2026-07-14-karpathy-llm-wiki-readme.md)

## Overview

LLM Wiki는 Andrej Karpathy가 제안한 지식 관리 개념으로, LLM이 원문 문서를 매번
검색하는 대신 구조화된 위키 페이지를 직접 작성·유지하는 시스템이다. 새 자료가
들어오면 모델이 요약, 상호 참조, 인덱스, 모순 추적을 갱신하므로 지식이 시간이
지날수록 축적된다. 이 저장소(두 번째 브레인) 자체가 이 개념의 구현체다.

## RAG와의 차이

RAG(Retrieval-Augmented Generation)는 **질의 시점**에 원문 조각(chunk)을 검색해
답을 합성한다. 반면 LLM Wiki는 **수집(ingest) 시점**에 합성을 수행해 내구성 있는
큐레이션된 마크다운 페이지로 저장한다. 위키 방식은 사람이 곁들여 큐레이션하며
점진적으로 좋아지는 지식의 복리화(compounding)에 강하고, RAG는 방대한 코퍼스에
대한 광범위 검색에 적합하다.

## 세 가지 핵심 작업

시스템은 수집(ingest: 자료를 모아 위키 페이지로 컴파일), 질의(query: 위키를
검색해 출처를 인용한 답변 제공), 점검(lint: 인덱스 무결성·링크·전반적 건강 상태
검증)의 세 작업으로 운영된다.

## 검증된 사례

원 저장소는 2026년 4월부터 지속 운영된 실제 지식 베이스(출처 99건에서 컴파일된
글 94편)에서 나온 워크플로를 Agent Skill 형태로 패키징한 것이며, Claude Code,
Cursor, Codex CLI, OpenCode 등 Agent Skills 호환 도구에서 사용할 수 있다.
