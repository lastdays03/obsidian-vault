---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/ai_tools
  - editor/cursor
  - productivity
Up: [[AI_Dev_Tools_MOC]]
Source: User Prompt
---

# Cursor

## Definition
**Cursor**는 2023년 말 출시된 VS Code 기반의 AI 코드 에디터입니다. VS Code를 포크하여 개발되었기 때문에 기존 확장성 및 단축키를 그대로 유지하면서도, 최신 LLM(Claude 4, GPT-4o, Gemini 2.5 등)을 편집기에 깊숙이 통합하여 프로젝트 전체를 이해하고 복합적인 코딩 작업을 수행하는 데 특화되어 있습니다.

## Key Features (2026)
- **Composer (Cmd/Ctrl + I)**: 자연어로 요청 시 수십 개의 파일을 한 번에 제안하고 수정하는 최강의 멀티 파일 편집 기능.
- **Tab Autocomplete**: 다음 코드 라인이나 전체 함수를 예측하여 제안하며, 멀티 라인 편집 및 리팩토링도 지원.
- **Project Deep Context**: 코드베이스 전체를 인덱싱하여 프로젝트의 구조와 맥락을 완벽하게 파악한 응답 제공.
- **Rules (Custom Instructions)**: 프로젝트별 코딩 컨벤션이나 스타일(예: "Always use TypeScript", "Prefer Tailwind")을 강제할 수 있는 시스템.
- **Multi-LLM Integration**: Claude 4, GPT-4o, Gemini 2.5 Pro 등 상황에 맞는 최신 모델을 자유롭게 선택하여 사용 가능.
- **Context Search (@-symbols)**: `@파일`, `@폴더`, `@웹`, `@docs` 등을 통해 필요한 외부 정보를 채팅창에 쉽게 추가.

## Comparison (2026)
| Tool               | Base               | Project Context | Automation Scope         | Pricing            |
| ------------------ | ------------------ | --------------- | ------------------------ | ------------------ |
| **Cursor**         | IDE (VS Code Fork) | **Excellent**   | **Very Wide (Composer)** | Free / $20 (Pro)   |
| **GitHub Copilot** | Extension          | Good            | Narrow (Line/Func)       | $10/mo             |
| **Claude Code**    | CLI (Terminal)     | Good            | Wide (Agentic)           | Subscription       |
| **OpenCode**       | CLI (Terminal)     | Good            | Wide (Agentic)           | Free (Open Source) |

## Quick Start
1. **Download**: [cursor.com](https://cursor.com)에서 설치 및 VS Code 설정 가져오기.
2. **AI Setup**: Settings → AI Providers에서 Gemini(무료 키 권장) 또는 Pro 모델 활성화.
3. **Usage**:
    - `Cmd + I` (Composer) 호출 후 작업 설명.
    - `Cmd + K` (Edit) 특정 영역 수정 요청.
    - `@` 기호로 핵심 파일 참조 추가.

## Links
- [[AI_Dev_Tools_MOC]]
- [[Tech_Stack_MOC]]
- [[Claude_Code_MOC]]
- [[OpenCode/OpenCode|OpenCode]]
- [[Gemini_CLI/Gemini_CLI|Gemini CLI]]
