# 개념 (Concept): MCP Notion Server

**태그**: #knowledge/concept #topic/AI_Dev_Tools #tech_stack/MCP #tool/productivity
**출처**: User Input

---

## 📖 정의 (Definition)
**MCP Notion Server**는 Notion이 공식적으로 제공하는 **Model Context Protocol (MCP)** 서버로, Claude Desktop과 같은 AI 에이전트가 사용자의 **Notion 워크스페이스에 실시간으로 접근**하여 페이지와 데이터베이스를 읽고, 쓰고, 검색하고, 업데이트할 수 있게 해주는 도구이다. 2026년 기준, 로컬 설치형보다는 **Hosted (원격)** 버전이 표준으로 자리 잡았다.

---

## 💡 예시 (Example)

### 주요 기능 (Tools)
- **페이지 관리**: `get_page` (내용 읽기), `create_page` (생성), `update_page` (수정), `search_pages` (검색)
- **데이터베이스**: `query_database` (필터링/정렬 조회), `create_database_entry` (아이템 추가)
- **블록/콘텐츠**: `append_block_children` (내용 추가), `markdown_conversion` (AI 친화적 포맷 변환)

### 설정 예시 (Claude Desktop + Hosted)
1. **Notion Integration 생성**: [My Integrations](https://www.notion.so/my-integrations)에서 Internal Integration 생성 후 토큰(`secret_...`) 발급.
2. **권한 부여**: 접근하려는 페이지/DB의 `connections` 메뉴에서 생성한 Integration 연결.
3. **Claude 연결**: `Settings` -> `Integrations` -> `Notion` 선택 후 로그인/허용 (자동 설정).

### 프롬프트 예시
> "내 Notion '할 일' 데이터베이스에서 오늘 마감인 항목들 목록으로 보여주고, 가장 중요한 3개 요약해줘"
> "새 페이지 만들어줘. 제목은 '2026 Q1 리뷰', 부모 페이지는 '연간 계획' 아래에, 내용은 이 아이디어들로 채워줘..."

---

## ⚖️ 비교 (Comparison)

| 특징            | Notion Hosted MCP (Official)       | Notion Local MCP (Opensource) | Notion AI             |
| :-------------- | :--------------------------------- | :---------------------------- | :-------------------- |
| **실행 방식**   | **Remote (Cloud)**                 | Local Process (npx)           | Built-in (In-App)     |
| **설정 난이도** | 매우 쉬움 (로그인 방식)            | 중간 (Env 설정 필요)          | 없음                  |
| **기능 범위**   | 전체 워크스페이스 제어 (읽기/쓰기) | 제한적 기능                   | 텍스트 생성/요약 위주 |
| **토큰 효율**   | **최적화됨 (Markdown)**            | JSON Raw Data (비효율)        | -                     |
| **상태 (2026)** | **표준 (Mainstream)**              | Deprecated (Sunset)           | 보조 도구             |

---

## 🔑 Key Insights
- **Hosted가 표준**: 설치가 필요 없고 API 업데이트가 즉시 반영되는 Hosted 버전이 사실상 표준이다.
- **강력한 연결성**: 단순 텍스트 생성을 넘어, 개인 지식 베이스(Notion)를 AI의 'Long-term Memory'처럼 활용할 수 있게 해준다.
- **권한 주의**: Integration에 부여된 권한은 AI에게 그대로 위임되므로, 민감한 정보가 있는 워크스페이스 공유 시 주의가 필요하다.
- **Markdown 최적화**: 노션의 복잡한 블록 구조를 AI가 이해하기 쉬운 Markdown으로 변환하여 토큰 소모를 줄이고 정확도를 높였다.

## 📚 References
- [Notion Developers - MCP Guide](https://developers.notion.com/)
- [Model Context Protocol - Notion](https://modelcontextprotocol.io/servers/notion)
