# 개념 (Concept): MCP GitHub Server

**태그**: #knowledge/concept #topic/AI_Dev_Tools #tech_stack/MCP
**출처**: User Input

---

## 📖 정의 (Definition)
**MCP GitHub Server**는 Claude Desktop과 같은 MCP 호환 클라이언트가 **GitHub 저장소와 직접 상호작용**할 수 있게 해주는 도구이다. 자연어로 저장소의 파일을 읽고 쓰거나, 이슈 및 PR을 관리하고, 브랜치를 생성하는 등 GitHub의 핵심 기능을 에이전트가 수행할 수 있도록 연결한다.

---

## 💡 예시 (Example)

### 주요 기능 (Tools)
- **저장소 탐색**: `list_repositories`, `get_repository`, `search_code`
- **파일 작업**: `read_file`, `create_or_update_file`, `create_branch`
- **협업 관리**: `create_issue`, `create_pull_request`, `comment_on_pr`

### 설정 예시 (Docker - 권장 표준)
2026년 기준, **공식 Docker 이미지**를 사용하는 것이 가장 안정적이고 표준화된 방식이다.

**1. Docker 실행 명령어 (테스트용)**
```bash
docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN=ghp_YOUR_TOKEN ghcr.io/github/github-mcp-server:latest
```

**2. Claude Desktop Config (`claude_desktop_config.json`)**
```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN=ghp_YOUR_TOKEN",
        "ghcr.io/github/github-mcp-server:latest"
      ]
    }
  }
}
```

### 설정 예시 (Local `npx` - 대안)
Docker 사용이 어렵거나 가볍게 테스트할 때 사용한다.
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_YOUR_TOKEN"
      }
    }
  }
}
```

### 프롬프트 예시
> "내 'awesome-project' 저장소의 README.md를 읽고, 마지막 섹션에 'Contributing' 가이드 추가해줘."
> "지난 7일 동안 main 브랜치 변경 사항 요약해줘."

---

## ⚖️ 비교 (Comparison)

| 특징            | Official Remote Server | Local Server (PAT)       | Community Version |
| :-------------- | :--------------------- | :----------------------- | :---------------- |
| **설정 난이도** | 매우 쉬움 (UI 클릭)    | 중간 (Config 파일 수정)  | -                 |
| **속도/안정성** | 빠름/안정적            | npx 실행 딜레이 존재     | 낮음              |
| **보안**        | OAuth 인증             | 토큰(PAT) 직접 관리 필요 | -                 |
| **추천 여부**   | ★★★★★                  | 원격 불가 시 대안        | 비추천            |

---

## 🔑 Key Insights
- **공식 권장**: 2026년 기준 GitHub 공식(`github/github-mcp-server`)이 표준이며, 레거시 안트로픽 버전은 사용이 지양된다.
- **보안 주의**: 로컬 사용 시 PAT 권한은 `repo`, `user` 등 필요한 최소 범위로 제한해야 하며, 90일 만료 주기를 관리해야 한다.
- **단일 서버 원칙**: 여러 버전의 GitHub MCP를 동시에 활성화하면 Claude가 도구 선택에 혼란을 겪을 수 있으므로 하나만 유지하는 것이 좋다.

## 📚 References
- [GitHub Official MCP Server Repository](https://github.com/github/github-mcp-server)
- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
