# 개념 (Concept): MCP Filesystem Server

**태그**: #knowledge/concept #topic/AI_Dev_Tools #tech_stack/MCP
**출처**: User Input

---

## 📖 정의 (Definition)
**MCP Filesystem Server**는 Claude Desktop과 같은 AI 클라이언트가 사용자의 **로컬 컴퓨터 파일 시스템**에 안전하게 접근하여 파일을 읽고, 쓰고, 관리할 수 있게 해주는 **Model Context Protocol (MCP)** 의 구현체이다. 웹 버전의 보안 제약을 넘어 로컬 환경에서의 작업을 가능하게 한다.

---

## 💡 예시 (Example)

### 주요 기능 (Tools)
- **탐색**: `list_directory` (폴더 목록 조회)
- **읽기/쓰기**: `read_file`, `write_file`, `append_to_file`
- **관리**: `create_directory`, `move_file`
- **검색**: `search_files`

### 설정 예시 (`claude_desktop_config.json`)
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/username/projects",
        "/Users/username/Desktop/allowed-folder"
      ]
    }
  }
}
```

### ⚠️ Docker 설정을 권장하지 않는 이유
다른 MCP 서버(GitHub, Tavily 등)와 달리, Filesystem Server는 **호스트 컴퓨터의 파일에 직접 접근**하는 것이 주 목적이다. Docker로 실행할 경우 다음과 같은 문제가 발생하여 비효율적이다.

1.  **복잡한 마운트 (Volume Mapping)**: 접근하려는 모든 폴더를 `-v` 옵션으로 일일이 매핑해야 한다. (예: `-v /Users/me/projects:/projects`)
2.  **경로 불일치**: Claude가 인식하는 경로(컨테이너 내부)와 실제 호스트 경로가 달라져, "바탕화면에 파일 만들어줘" 같은 자연어 명령이 실패하거나 엉뚱한 곳에 실행될 수 있다.
3.  **권한 충돌 (Permission)**: 컨테이너 내부의 Root/User 권한과 호스트 OS의 파일 권한이 충돌하여 쓰기 작업이 실패하는 경우가 잦다.

👉 **결론**: Filesystem Server만큼은 **Local (`npx`) 방식**이나 **직접 실행**이 가장 빠르고 정확하다.

---

## ⚖️ 비교 (Comparison)

| 특징          | MCP Filesystem Server             | Claude Web Interface              |
| :------------ | :-------------------------------- | :-------------------------------- |
| **실행 환경** | 로컬 데스크탑 (Claude Desktop)    | 웹 브라우저                       |
| **파일 접근** | 허용된 로컬 폴더 직접 접근 가능   | 접근 불가 (파일 업로드만 가능)    |
| **보안 모델** | 사용자 승인 기반 폴더별 권한 부여 | 서버 측 샌드박스 실행             |
| **주요 용도** | 대량 리팩토링, 프로젝트 구조 분석 | 일반적인 질의응답, 단일 파일 분석 |

---

## 🔑 Key Insights
- **보안 중심 설계**: 루트(`/`) 경로 접근 금지, 사용자 승인 다이얼로그(Allow/Reject)를 통한 철저한 통제 하에 작동한다.
- **데스크탑 전용**: 보안 및 OS 접근 권한 문제로 인해 웹 버전에서는 사용할 수 없으며, Claude Desktop이나 MCP 지원 에디터에서만 작동한다.
- **활용성**: 단순 파일 읽기를 넘어 프로젝트 전체 구조 분석, 대규모 코드 리팩토링, 자동화된 문서 정리 등 "에이전트"로서의 활용 범위를 대폭 확장시킨다.

## 📚 References
- [Official MCP Filesystem Server Repository](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)
- [Anthropic Model Context Protocol Introduction](https://www.anthropic.com/news/model-context-protocol)
