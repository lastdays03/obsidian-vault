---
tags: [concept/ai, methodology/knowledge-engineering]
Up: [[AI_Concepts_MOC]]
---

# Docs-to-Skill

> **Docs-to-Skill**은 최신 라이브러리나 API의 공식 문서를 실시간으로 수집(Ingestion)하여, AI 에이전트가 이해하고 실행할 수 있는 **Skill (절차적 지식)** 포맷으로 변환하는 자동화 파이프라인입니다.

## 📖 Definition
LLM(Large Language Model)의 가장 큰 약점은 학습 데이터의 시의성(Cut-off Date)입니다.
**Docs-to-Skill**은 이 문제를 해결하기 위해, 다음과 같은 프로세스를 수행합니다.
1.  **Crawl**: `browser-tools` (MCP)를 사용하여 특정 URL의 기술 문서를 읽습니다.
2.  **Synthesize**: 문서에서 핵심 개념, 설치 방법, 기본 예제를 추출합니다.
3.  **Generate**: 이를 `SKILL.md` 포맷으로 변환하고 `.claude/skills`에 저장합니다.
4.  **Inject**: 이후 작업부터 AI는 이 Skill을 통해 최신 지식을 활용합니다.

## 💻 Example
**Bun v1.1**이 막 출시되었고, Claude가 이를 모른다고 가정할 때:

```bash
# Meta-Prompt Execution
claude -p "
Visit 'https://bun.sh/docs' via browser_tool. 
Read 'Quick Start' and 'HTTP Server'.
Create a skill named 'bun-expert' that explains how to set up a server using the new API.
"
```

## 🆚 Comparison
| 구분 | RAG (Retrieval-Augmented Generation) | Docs-to-Skill |
| :--- | :--- | :--- |
| **방식** | 쿼리 시점에 관련 청크 검색 | 사전에 지식을 Skill로 컴파일 |
| **활용** | 일회성 답변 생성 | 반복적인 코딩 작업에 적용 |
| **지속성** | 휘발성 컨텍스트 | 영속적 파일 (`SKILL.md`) |

## 🔗 Connected Concepts
- [[RAG]]
- [[Model_Context_Protocol]]
- [[Knowledge Engineering]]

Source: [[04_Future_Proofing]]
