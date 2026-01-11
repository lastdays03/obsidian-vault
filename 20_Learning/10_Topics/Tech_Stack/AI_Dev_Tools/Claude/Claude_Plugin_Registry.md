---
tags:
  - tool/claude-code
  - plugin/registry
  - ai_agent
source: "Community & Official Docs (2026.01)"
Up: [[Claude_Code_Handbook]]
---

# Claude Code Plugin Registry (2026 Edition)

> **문서 개요**: Claude Code의 생산성을 극대화하는 **Top 20 필수 플러그인** 목록입니다.
> **💡 중요 참고**: `/plugin` 명령어는 사용하시는 Claude Code 래퍼(Wrapper) 버전에 따라 다를 수 있습니다. 아래 설치 명령어는 **표준 MCP(Model Context Protocol) 설정**을 기준으로 가장 확실한 방법을 함께 기재했습니다.

---

## 🚀 Essential (Must-Have)

| No    | 이름 (Name)             | 설명 (Description)                           | 설치 (Installation / MCP Config)                        | Link                                                  |
| :---- | :-------------------- | :----------------------------------------- | :---------------------------------------------------- | :---------------------------------------------------- |
| **1** | **Superpowers**       | **[강력 추천]** TDD, 디버깅, 계획 수립 등 핵심 스킬 라이브러리. | `/plugin install superpowers@superpowers-marketplace` | [GitHub](https://github.com/obra/superpowers)         |
| **2** | **Frontend-design**   | 고품질 UI/디자인 생성 에이전트.                        | `/plugin install frontend-design`                     | [Official](https://github.com/anthropics/claude-code) |
| **3** | **PR Review Toolkit** | 자동 PR 코드 리뷰 툴킷.                            | `/plugin install pr-review-toolkit`                   | [Official](https://github.com/anthropics/claude-code) |
| **4** | **Feature-dev**       | 기능 개발 워크플로우 (Anthropic 표준).                | `/plugin install feature-dev`                         | [Official](https://github.com/anthropics/claude-code) |
| **5** | **Code-review**       | 자신감 기반 코드 리뷰 에이전트.                         | `/plugin install code-review`                         | [Official](https://github.com/anthropics/claude-code) |

## 🛠️ Development & Ops

| No     | 이름 (Name)            | 설명 (Description)                       | 설치 (Installation / MCP Config)             | Link                                                  |
| :----- | :------------------- | :------------------------------------- | :----------------------------------------- | :---------------------------------------------------- |
| **6**  | **Repomix-mcp**      | 코드베이스 분석 및 패키징 MCP 서버.                 | `/plugin install repomix-mcp@repomix`      | [Repomix](https://github.com/yamadashy/repomix)       |
| **7**  | **Repomix-commands** | Repomix 슬래시 명령어 확장.                    | `/plugin install repomix-commands@repomix` | [Repomix](https://github.com/yamadashy/repomix)       |
| **8**  | **Repomix-explorer** | AI 기반 리포지토리 자연어 탐색기.                   | `/plugin install repomix-explorer@repomix` | [Repomix](https://github.com/yamadashy/repomix)       |
| **9**  | **Agent-sdk-dev**    | Agent SDK 프로젝트 설정/검증 도구.               | `/plugin install agent-sdk-dev`            | [Official](https://github.com/anthropics/claude-code) |
| **10** | **Migration Tool**   | 모델 마이그레이션 도구 (Sonnet/Opus 업그레이드).      | `/plugin install migration-tool`           | [Official](https://github.com/anthropics/claude-code) |
| **11** | **Ralph Wiggum**     | 반복적 AI 루프 (무한 루프 방지). *Superpowers 권장* | `npx -y @smith/ralph-wiggum`               | [Superpowers](https://github.com/obra/superpowers)    |

## 🧩 Integration & Utility (MCP Core)

| No     | 이름 (Name)         | 설명 (Description)                       | 설치 (Installation / MCP Config)                 | Link                                                                                       |
| :----- | :------------------ | :--------------------------------------- | :----------------------------------------------- | :----------------------------------------------------------------------------------------- |
| **12** | **Context7**        | 최신 문서 연결 (Puppeteer 활용).         | `npx -y @modelcontextprotocol/server-puppeteer`  | [MCP/puppeteer](https://github.com/modelcontextprotocol/servers/tree/main/src/puppeteer)   |
| **13** | **GitHub MCP**      | GitHub 이슈/PR 실시간 연동.              | `npx -y @modelcontextprotocol/server-github`     | [MCP/github](https://github.com/modelcontextprotocol/servers/tree/main/src/github)         |
| **14** | **Supabase MCP**    | Supabase DB 및 프로젝트 제어 (Postgres). | `npx -y @modelcontextprotocol/server-postgres`   | [MCP/postgres](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres)     |
| **15** | **Security-review** | 보안 취약점 분석 (Snyk 연동).            | `npx -y @modelcontextprotocol/server-snyk`       | [MCP Servers](https://github.com/modelcontextprotocol/servers)                             |
| **16** | **DevOps Auto**     | CI/CD (GitLab/Kubernetes) 연동.          | `npx -y @modelcontextprotocol/server-gitlab`     | [MCP/gitlab](https://github.com/modelcontextprotocol/servers/tree/main/src/gitlab)         |
| **17** | **Doc Generator**   | 자동 문서 생성 (Filesystem 접근).        | `npx -y @modelcontextprotocol/server-filesystem` | [MCP/filesystem](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem) |

## 🔬 Experimental & Advanced

| No     | 이름 (Name)     | 설명 (Description)                   | 설치 (Installation / MCP Config)                 | Link                                                                                       |
| :----- | :-------------- | :----------------------------------- | :----------------------------------------------- | :----------------------------------------------------------------------------------------- |
| **18** | **Playwright**  | 브라우저 테스팅 자동화.              | `npx -y @modelcontextprotocol/server-playwright` | [MCP/playwright](https://github.com/modelcontextprotocol/servers/tree/main/src/playwright) |
| **19** | **Obsidian**    | Obsidian 노트/Vault 연동.            | `npx -y mcp-obsidian`                            | [Community](https://github.com/calclavia/mcp-obsidian)                                     |
| **20** | **Multi-Agent** | 고급 멀티 에이전트 (LangGraph 권장). | `/plugin install swarm-intelligence`             | [LangGraph](https://github.com/langchain-ai/langgraph-mcp)                                 |

---

## 💡 핵심 설치 포인트 (Core Installation Points)

### 1. 공식 MCP 서버 활용 (Official Servers)
GitHub, Supabase, Playwright 등은 별도의 서드파티 플러그인보다 **Anthropic 공식 MCP 서버 리포지토리(`modelcontextprotocol/servers`)**의 것을 사용하는 것이 가장 안정적이고 보안상 안전합니다.
- 설치 시 `npx` 명령어를 사용하거나 Claude Desktop/Code 설정 파일(`claude_code_config.json`)의 `mcpServers` 항목에 추가하십시오.

### 2. Repomix (Code Analysis)
일본 개발자 `yamadashy`가 만든 **Repomix**는 현재 Claude Code 사용자들이 코드베이스 전체를 분석할 때 가장 많이 쓰는 필수 툴입니다.

### 3. 대체 추천 (Alternatives)
- **Ralph Wiggum**: 특정 스크립트보다는 `Superpowers` 내의 `think` 도구를 사용하는 것이 더 강력합니다.
- **Context7**: 별도 플러그인 대신 `Puppeteer` MCP 서버를 통해 웹 문서를 직접 긁어오는 것이 정석입니다.
