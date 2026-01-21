---
tags:
  - MCP
  - Obsidian
  - Claude
  - Guide
created: 2026-01-21
---

# Obsidian MCP Server 연결 가이드 (Claude Desktop)

옵시디언(Obsidian)을 Claude Desktop 등의 AI 에이전트와 연결하기 위한 MCP(Model Context Protocol) 서버 설치 및 설정 방법입니다.

가장 권장되는 방식은 **`uvx`**를 이용한 간편 설치법이며, 사전에 옵시디언 내부 **Local REST API** 플러그인 설정이 필요합니다.

## 1단계: 옵시디언 사전 설정 (Local REST API 설치)

MCP 서버가 옵시디언 데이터에 접근할 수 있도록 통로를 열어줘야 합니다.

1. **옵시디언 실행** 후 `설정(Settings)` -> `커뮤니티 플러그인(Community plugins)`으로 이동합니다.
2. `탐색(Browse)` 버튼을 누르고 **"Local REST API"**를 검색하여 설치 및 활성화합니다.
3. 플러그인 설정 화면에서 **API Key**를 복사해 둡니다. (나중에 설정 파일에 입력해야 합니다.)
4. 보안을 위해 `Enable Non-localhost Access`는 가급적 끄고 사용하세요(기본값 권장).

---

## 2단계: MCP 서버 설치 및 연결

Claude Desktop 앱을 기준으로 설명합니다.

### 방법 A: `uvx`를 이용한 간편 설치 (권장)

Python의 패키지 관리 도구인 `uv`가 설치되어 있어야 합니다. (터미널에서 `pip install uv`로 설치 가능)

1. Claude Desktop의 설정 파일(`claude_desktop_config.json`)을 엽니다.
    * **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
    * **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`

2. `"mcpServers"` 항목 안에 아래 내용을 추가합니다.

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "uvx",
      "args": ["mcp-obsidian"],
      "env": {
        "OBSIDIAN_API_KEY": "여러분의_API_KEY_입력"
      }
    }
  }
}
```

### 방법 B: `npx`를 이용한 설치 (Node.js 사용자)

Node.js가 설치되어 있다면 아래 설정을 사용합니다.

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["-y", "@mseep/obsidian-mcp-server"],
      "env": {
        "OBSIDIAN_API_KEY": "여러분의_API_KEY_입력",
        "OBSIDIAN_PORT": "27124"
      }
    }
  }
}
```

---

## 3단계: 설치 확인 및 사용

1. **Claude Desktop 재시작:** 설정을 변경한 후 반드시 앱을 완전히 종료했다가 다시 실행하세요.
2. **연결 확인:** 채팅창 우측 하단의 🔌(플러그 아이콘)을 클릭했을 때 `obsidian` 서버가 초록색 불(Running)로 표시되면 성공입니다.
3. **명령어 예시:**
    * "내 옵시디언의 '업무' 폴더에 있는 최근 노트 3개 요약해줘."
    * "오늘 회의 내용을 '2026-01-21 회의'라는 제목으로 새 노트 만들어줘."

---

## 주의사항

* 옵시디언이 **실행 중**이어야 API를 통해 데이터를 주고받을 수 있는 서버가 많습니다. (일부 직접 파일 접근 방식 제외)
* API 키를 외부에 노출하지 않도록 주의하세요.
