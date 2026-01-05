---
tags: [concept/ai, protocol/mcp]
Up: [[AI_Concepts_MOC]]
---

# Model Context Protocol

> **Model Context Protocol (MCP)**는 AI 모델이 로컬 및 원격 데이터 소스(파일 시스템, 데이터베이스, API)와 안전하게 상호작용할 수 있도록 하는 **개방형 표준 인터페이스**입니다.

## 📖 Definition
지금까지 LLM은 외부 세상과 단절된 "뇌"에 불과했습니다. **MCP**는 이 뇌에 "손과 발"을 달아주는 기술입니다.
JSON-RPC 2.0 기반의 프로토콜을 사용하여, AI(Client)와 도구(Server) 간의 통신을 추상화합니다. 덕분에 AI 모델이 바뀔 때마다 도구를 새로 구현할 필요 없이, 표준 드라이버만 교체하면 됩니다.

## 💻 Structure
- **MCP Host**: AI 애플리케이션 (예: Claude Desktop, Zed IDE).
- **MCP Client**: 호스트 내에서 서버와 통신하는 모듈.
- **MCP Server**: 실제 리소스(Resource), 프롬프트(Prompt), 도구(Tool)를 제공하는 주체.

## 🆚 Comparison
| 특징 | OpenAI Functions | MCP |
| :--- | :--- | :--- |
| **범위** | 특정 모델 종속적 | 모델/플랫폼 중립적 |
| **연결성** | 1:1 연결 (직접 구현) | N:M 연결 (표준 드라이버) |
| **확장성** | API 호출 위주 | 파일, DB, 로그 등 모든 리소스 |

## 🔗 Connected Concepts
- [[LLM Tools]]
- [[API Integration]]
- [[Claude_Code_MOC]]

Source: [[02_MCP_Ecosystem]]
