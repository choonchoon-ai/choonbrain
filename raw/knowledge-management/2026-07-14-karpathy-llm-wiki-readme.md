# karpathy-llm-wiki — Agent Skill for LLM-maintained knowledge wikis

> Source: https://github.com/Astro-Han/karpathy-llm-wiki
> Collected: 2026-07-14
> Published: Unknown

---

원문을 충실하게 보존한다. 아래는 저장소 README에서 수집한 내용이다.

{이 아래에 원문 내용...}

## Overview

**karpathy-llm-wiki** is an installable Agent Skills component for constructing
knowledge systems where LLMs maintain structured wiki pages rather than repeatedly
searching raw documents. The tool ingests sources into `raw/`, compiles durable
knowledge into `wiki/`, answers queries with citations, and validates wiki consistency.

## Core Concept

An LLM wiki differs from traditional wikis through automated LLM-driven maintenance.
The model updates summaries, cross-references, indices, and contradiction tracking
as new materials arrive.

## Three Primary Operations

1. **Ingest**: Collects sources and compiles them into wiki pages
2. **Query**: Searches wiki pages and provides cited answers
3. **Lint**: Validates index integrity, links, and overall health

## Comparison with RAG

LLM wikis focus on durable, curated markdown pages where synthesis occurs during
ingestion, whereas RAG systems retrieve across raw chunks at query time. Wikis excel
at compounding knowledge; RAG suits broad corpus retrieval.

## Production Context

The approach stems from a functioning knowledge base containing 94 articles compiled
from 99 sources, maintained continuously since April 2026.

## Installation

```
npx add-skill Astro-Han/karpathy-llm-wiki
```

## Supported Tools

Compatible with Claude Code, Cursor, Codex CLI, OpenCode, and other Agent
Skills-compliant platforms.

## Quick Workflow

Users ingest sources via URLs, files, or pasted content; query the wiki for
synthesized answers; and run linting to maintain health.
