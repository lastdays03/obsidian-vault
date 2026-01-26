---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/ai_tools
  - agent/coding
  - open_source
Up: [[AI_Dev_Tools_MOC]]
Source: User Prompt
---

# OpenCode

## Definition
**OpenCode**는 100% 오픈소스 AI 코딩 에이전트로, 개발자가 터미널, IDE, 데스크톱에서 AI를 활용해 코드를 작성, 수정, 리팩토링 및 디버깅할 수 있게 돕는 도구입니다. 2026년 기준 Claude Code와 GitHub Copilot의 강력한 오픈소스 대안으로 자리 잡았습니다.

## Key Features (2026)
- **100% Open Source**: 벤더 락인 없이 자유로운 커스텀 및 포크 가능.
- **Multi-LLM Support**: OpenAI, Claude, Gemini, Groq뿐만 아니라 Ollama를 통한 **로컬 모델** 연동 지원.
- **Agentic Capabilities**: 파일 읽기/쓰기, 쉘 명령 실행, Git 커밋/푸시 자동화 등 "진짜 작업 대행" 수행.
- **Multi-Platform**: 터미널(TUI), 데스크톱 앱, VS Code 확장 프로그램 지원.
- **Privacy First**: 컨텍스트 데이터를 서버로 전송하지 않고 로컬에서 처리 가능.

## Comparison (2026)
| Tool            | Price           | Open Source | Terminal Support | Best Case                             |
| --------------- | --------------- | ----------- | ---------------- | ------------------------------------- |
| **OpenCode**    | Free (API only) | **Yes**     | Excellent        | Developers seeking freedom & low cost |
| **Claude Code** | Subscription    | No          | Good             | Native Claude optimization            |
| **Cursor**      | $20/mo          | No          | IDE focus        | Seamless IDE experience               |

## Quick Start
```bash
# Install via terminal
curl -fsSL https://opencode.ai/install | bash

# Set API Key
opencode config set anthropic_api_key sk-...

# Start coding
opencode "Add JWT auth middleware to this project"
```

## Links
- [[AI_Dev_Tools_MOC]]
- [[Tech_Stack_MOC]]
- [[Claude_Code]] (Alternative)
- [[Antigravity]] (Advanced Agent)
