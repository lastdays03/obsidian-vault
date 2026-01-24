---
tags: [n8n, mcp, guide, ai-agent, automation]
date: 2026-01-23
status: active
Up: [[n8n]]
---

# n8n과 MCP(Model Context Protocol) 서버 연동 가이드

n8n에서 **MCP**는 2025년 이후 크게 업데이트된 기능으로, **AI 에이전트(Claude Desktop, Cursor, Lovable, Windsurf 등)** 가 n8n 워크플로우와 도구를 직접 호출할 수 있게 해주는 **표준 프로토콜**입니다.
Anthropic(Claude 개발사)이 주도한 오픈 표준으로, **n8n을 MCP 서버**로 만들거나 **n8n을 MCP 클라이언트**로 만들어 외부 MCP 서버를 호출할 수 있습니다.

## MCP 연동의 두 가지 주요 패턴

| 패턴                              | 설명                                                                   | n8n 역할       | 대표적인 사용 사례                                             | 필요한 n8n 버전                  |
| --------------------------------- | ---------------------------------------------------------------------- | -------------- | -------------------------------------------------------------- | -------------------------------- |
| **n8n as MCP Server** (가장 인기) | n8n 워크플로우/도구를 AI가 직접 호출                                   | **서버**       | Claude Desktop에서 n8n 워크플로우 실행, AI가 자동으로 n8n 제어 | 1.88.0 이상 (2025년 4월 이후)    |
| **n8n as MCP Client**             | n8n이 외부 MCP 서버(예: Google Calendar MCP, PostgreSQL MCP 등)를 호출 | **클라이언트** | AI Agent가 외부 도구(Atlassian, GitHub, Notion 등) 사용        | 1.88.0 이상 + 커뮤니티 노드 설치 |

대부분의 사용자는 **n8n을 MCP 서버**로 만들어 **Claude Desktop / Cursor** 등에서 n8n을 AI로 제어하는 방식을 선호합니다.

## 1. n8n을 MCP 서버로 만드는 방법 (가장 추천)

n8n 1.88.0 이상 버전부터 **공식 MCP Server Trigger** 노드가 추가되어 매우 쉽게 구현할 수 있습니다.

### 방법 A: 인스턴스 전체 MCP 활성화 (가장 간단, 2025년 말~2026년 추천 방식)

1. **n8n 설정 → Instance-level MCP** 메뉴로 이동
2. **Enable MCP access** 토글 ON (인스턴스 관리자 권한 필요)
3. 자동 생성되는 **MCP URL** (Test / Production) 확인
   - `Test URL`: 로컬 테스트용
   - `Production URL`: 실제 Claude Desktop 등에서 사용할 URL
4. **워크플로우별로 MCP 노출** → 워크플로우 설정에서 **Expose as MCP Tool** 토글 ON
5. Claude Desktop / Cursor 설정에서 Production URL 입력 → 완료!

### 방법 B: 개별 워크플로우로 MCP Server Trigger 노드 사용 (더 세밀한 제어)

1. 새 워크플로우 생성
2. **MCP Server Trigger** 노드 추가 (`Core Nodes` > `LangChain` > `MCP Server Trigger`)
3. MCP Server Trigger 노드 설정
   - **Authentication**: Bearer Token 추천 (보안)
     - Token 생성 → `n8n Credentials` > `HTTP Request` > `Bearer Auth`
   - **Tools** → 바로 아래에 **Tool 노드**들 연결 (HTTP Request, Code, Google Calendar 등)
4. 워크플로우 활성화 → 노드 상단에 **Test URL** / **Production URL** 자동 생성
5. **Claude Desktop** 설정 예시:
   ```json
   {
     "mcpServers": {
       "my-n8n": {
         "url": "https://your-n8n-domain.com/mcp/production/...",
         "auth": {
           "type": "bearer",
           "token": "your-bearer-token"
         }
       }
     }
   }
   ```
6. Claude에게 "n8n 워크플로우 목록 보여줘" 또는 "내 n8n에서 이메일 보내" 같은 명령 → 동작 확인!

## 2. n8n을 MCP 클라이언트로 사용하는 방법

외부 MCP 서버(예: Atlassian MCP, PostgreSQL MCP, GitHub MCP 등)를 n8n AI Agent에서 호출합니다.

1. **커뮤니티 노드 설치** (n8n 1.50 이상)
   ```bash
   docker exec -it n8n npm install n8n-nodes-mcp
   ```
   또는 `Settings` → `Community Nodes` → `n8n-nodes-mcp` 검색 & 설치

2. 워크플로우에서 **MCP Client Tool** 노드 추가 (`LangChain` > `Tools` > `MCP Client Tool`)
3. 설정
   - **SSE Endpoint**: 외부 MCP 서버의 SSE URL (예: `https://mcp.atlassian.com/sse`)
   - **Authentication**: Bearer / Header / OAuth2 / None
4. **AI Agent** 노드에 MCP Client Tool 연결 → 이제 AI가 외부 MCP 도구 사용 가능!

## 3. 추천 MCP 서버 예시 (n8n과 함께 쓰기 좋은 것들)

| MCP 서버 이름           | 기능                | n8n에서 쓰기 좋은 이유           |
| ----------------------- | ------------------- | -------------------------------- |
| **PostgreSQL MCP**      | DB 조회/수정        | n8n AI Agent가 DB 직접 조작 가능 |
| **Google Calendar MCP** | 일정 관리           | AI가 캘린더 자동 생성/조회       |
| **FileSystem MCP**      | 로컬 파일 읽기/쓰기 | n8n 서버 파일 직접 제어          |
| **Atlassian MCP**       | Jira / Confluence   | 기업용 워크플로우 자동화         |
| **Supabase MCP**        | Supabase CRUD + RAG | AI + 벡터 DB + n8n 완벽 조합     |

## 4. 자주 발생하는 문제 & 해결

| 문제              | 원인                    | 해결                                                 |
| ----------------- | ----------------------- | ---------------------------------------------------- |
| MCP URL 접속 안됨 | n8n이 로컬/내부망       | **ngrok** 또는 **Cloudflare Tunnel** 사용 (5분 설정) |
| 인증 실패         | Bearer Token 잘못됨     | n8n Credentials에서 Bearer Auth 새로 생성            |
| Tool이 안 보임    | 워크플로우 활성화 안됨  | 모든 연결된 Tool 노드 + MCP Trigger 활성화           |
| Rate Limit        | Claude가 너무 자주 호출 | n8n → Rate Limit Trigger 노드 추가                   |

## 5. 실전 템플릿 추천

- [Build your own N8N workflows MCP server](https://n8n.io/workflows/3770) → n8n 워크플로우 전체를 MCP로 노출
- [Build your own PostgreSQL MCP server](https://n8n.io/workflows/3631) → DB MCP 서버
- [Build your own FileSystem MCP server](https://n8n.io/workflows/3630) → 파일 시스템 MCP

## 결론 & 추천 시작점

- **초보** → **Instance-level MCP** 켜고 Claude Desktop에 Production URL 넣기 (5분 완성)
- **고급** → MCP Server Trigger + Tool 노드 조합으로 세밀한 MCP 서버 구축
- **AI Agent 만들기** → n8n AI Agent + MCP Client Tool + 외부 MCP 서버 조합
