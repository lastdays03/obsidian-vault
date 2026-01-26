---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/ai_tools
  - agent/coding
  - google/gemini
  - open_source
Up: [[AI_Dev_Tools_MOC]]
Source: User Prompt
---

# Gemini CLI

## Definition
**Gemini CLI**는 2025년 6월 Google이 공식 출시한 오픈소스 AI 에이전트로, 터미널에서 Google Gemini 모델(2.5 Pro/Flash 등)을 호출하여 코딩, 디버깅, 배포 및 워크플로우 자동화를 수행하는 명령줄 도구입니다. 2026년 기준 개발자들 사이에서 필수적인 AI 코딩 동반자로 자리 잡았습니다.

## Key Features (2026)
- **Official Open Source**: Google이 직접 관리하며 커뮤니티 확장이 자유로운 오픈소스 프로젝트.
- **TUI Focus**: 터미널 UI 기반의 인터랙티브 세션으로 로컬 개발 환경에 최적화.
- **Full Agentic Flow**: 파일 편집, Git 자동 커밋/푸시, 쉘 명령 실행 등 실제 작업을 대행.
- **MCP Integration**: Model Context Protocol을 기본 지원하여 n8n이나 외부 도구와 강력하게 연동.
- **Checkpointing**: 작업 도중 상태를 저장하고 복구할 수 있어 대규모 리팩토링에 안정적.
- **Extensions**: 2025년 10월 도입된 확장 시스템으로 GitHub Actions 연동 및 SRE 자동화 지원.

## Gemini CLI vs Others (2026)
| Tool            | Core Model            | Open Source | Price           | Best Case                         |
| --------------- | --------------------- | ----------- | --------------- | --------------------------------- |
| **Gemini CLI**  | Google Gemini         | **Yes**     | Free (API only) | Gemini 최적화 & Google Cloud 연동 |
| **Claude Code** | Anthropic Claude      | No          | Subscription    | Claude 성능 우선                  |
| **OpenCode**    | Multi (Claude/OpenAI) | **Yes**     | Free (API only) | 최고의 자유도 & 로컬 모델(Ollama) |

## Quick Start
```bash
# Install
curl -fsSL https://geminicli.com/install | bash

# Set API Key (Google AI Studio)
gemini config set api_key YOUR_GEMINI_API_KEY

# Basic Usage
gemini "Add a README.md to this project with project overview"
gemini debug # Debug current project
```

## Links
- [[AI_Dev_Tools_MOC]]
- [[Tech_Stack_MOC]]
- [[Claude_Code_MOC]]
- [[OpenCode]]
- [[n8n_Developer_Automation_2026]]
