---
tags: 
  - "#tool/mcp"
  - "#ai/context"
  - "#dev/productivity"
  - "#claude/code"
---

# Context7 MCP Server (컨텍스트 7)

**상태**: #learning/note
**상위 문서**: [[AI_Dev_Tools_MOC]]

---

## 🧐 개요 (Overview)

**Context7 (컨텍스트 7)**은 Claude Code 및 Claude Desktop 사용자를 위한 **가장 실용적인 MCP 서버** 중 하나입니다.  
개발 시 필요한 **최신 라이브러리, 패키지, API 문서**를 LLM(Claude)이 자동으로 검색하고 참조하여, **환각(Hallucination)을 줄이고 정확한 코드를 생성**하도록 돕습니다.

## 🎯 주요 목적 (Why use it?)
1.  **지식 Cutoff 해결**: 2025년 이후 업데이트된 최신 패키지(Next.js 15, React 19, FastAPI 0.115+ 등)의 문서를 Claude에게 제공.
2.  **생산성 향상**: 매번 웹 검색이나 문서 탭 전환 없이, 프롬프트에 바로 최신 Docs와 코드 예제를 주입.
3.  **환각 최소화**: 구버전 API 사용으로 인한 에러 방지.
4.  **강력한 도구 연동**: Cursor, Claude Code, VS Code + Cline 등에서 필수 도구로 활용.

## ⚙️ 동작 원리 (Process)
1.  사용자가 프롬프트 입력 (예: "Next.js App Router로 페이지 만들어줘")
2.  Claude가 **Context7 MCP 도구** 호출 (`resolve-library-id` → `get-library-docs`)
3.  Context7이 **Upstash 서버/캐시**에서 최신 공식 문서 및 코드 스니펫 로드
4.  Claude 컨텍스트에 내용을 **동적 로드 (Lazy Loading)**
5.  최신 문서를 바탕으로 **정확한 코드 생성**

## 📦 설치 및 사용법 (Installation)

### 1. Claude Code CLI (추천)
```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp --api-key YOUR_API_KEY
```
*API 키 발급: [Context7.com](https://context7.com) (무료/유료)*

### 2. Claude Desktop Config
`claude_desktop_config.json`에 추가:
```json
{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/mcp",
      "headers": {
        "CONTEXT7_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

## ⚖️ 장단점 (Pros & Cons)

| 장점 | 단점/주의점 |
| :--- | :--- |
| **정확성**: 최신 Docs 100% 반영, 환각 제로에 도전 | **비용**: 무료 티어 제한 (API 호출/토큰) |
| **속도**: 캐시 + Lazy Loading으로 빠름 | **지연**: 피크타임 발생 가능 |
| **효율**: Tool Search로 컨텍스트 절약 | **경합**: 여러 MCP 서버 동시 사용 시 컨텍스트 압박 |
| **특화**: VisionCraft보다 문서 검색에 강점 | **한글**: 한국어 문서 직접 지원은 약함 (영어 Docs 중심) |

## 💡 실전 팁 (Best Practices)
- **FastAPI, Next.js, LangChain** 등 변화가 빠른 라이브러리 작업 시 필수.
- 프롬프트에 `use context7`을 명시하여 호출 유도 가능.
- **Obsidian + MCP 조합**으로 노트에 최신 코드 스니펫 자동 삽입 활용 가능.

---
**요약**: Context7 MCP는 **"Claude야, 네가 모르는 최신 문서 지금 당장 가져와!"**라고 명령하는 버튼과 같습니다.
