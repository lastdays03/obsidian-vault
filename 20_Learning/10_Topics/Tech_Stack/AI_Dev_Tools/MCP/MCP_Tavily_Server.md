# 개념 (Concept): MCP Tavily Server

**태그**: #knowledge/concept #topic/AI_Dev_Tools #tech_stack/MCP #tool/search
**출처**: User Input

---

## 📖 정의 (Definition)
**MCP Tavily Server**는 Claude Desktop과 같은 MCP 호환 클라이언트가 **Tavily AI의 검색 엔진**을 활용하여 실시간 웹 정보를 검색하고 고품질 콘텐츠를 추출할 수 있게 해주는 도구이다. 단순한 URL 링킹을 넘어, 에이전트에 최적화된 직접 답변(Direct Answer)과 정제된 컨텍스트를 제공하여 할루시네이션을 줄이고 정보의 정확성을 높인다.

---

## 💡 예시 (Example)

### 주요 기능 (Tools)
- **`tavily-search`**: 쿼리 기반 웹 검색. 검색 의도에 맞춘 요약, 직접 답변, 관련 링크 목록을 반환한다.
- **`tavily-extract`**: 특정 URL의 핵심 본문을 마크다운 형태로 깨끗하게 추출한다. (Ads/Nav 제거)
- **`tavily-crawl`** (확장): 사이트 구조를 파악하거나 깊이 있는 정보 수집을 수행한다.



### 설정 예시 Reference (Remote URL - 권장)
가장 안정적이고 추천되는 방식은 **Remote MCP** 연결이다.
- **URL**: `https://mcp.tavily.com/mcp/?tavilyApiKey=tvly-YOUR_API_KEY`
- **방법**: Claude Desktop 설정 -> `Developer` -> `Edit Config` (또는 UI의 Add Remote MCP)

### 설정 예시 (Local `npx` - 간편 실행)
```json
{
  "mcpServers": {
    "tavily-mcp": {
      "command": "npx",
      "args": ["-y", "tavily-mcp"],
      "env": {
        "TAVILY_API_KEY": "tvly-YOUR_API_KEY"
      }
    }
  }
}
```

---

## ⚖️ 비교 (Comparison)

| 특징            | MCP Tavily Server                            | Claude Built-in Search      | Google/Perplexity MCP |
| :-------------- | :------------------------------------------- | :-------------------------- | :-------------------- |
| **최적화 대상** | **AI Agent (기계 가독성)**                   | 일반 사용자 (인간 가독성)   | 하이브리드            |
| **콘텐츠 품질** | 노이즈 제거된 순수 텍스트/마크다운 추출 우수 | 요약 위주, 원문 접근 제한적 | Varying               |
| **비용**        | 월 1,000 크레딧 무료 (개인 충분)             | Claude Pro 구독 비용에 포함 | 별도 API 키 필요      |
| **속도**        | 매우 빠름 (API 호출)                         | 보통 (브라우징 렌더링 포함) | 빠름                  |

---

## 🔑 Key Insights
- **"검색이 아닌 '지식 확보' 도구"**: 단순 링크 나열이 아니라, LLM이 이해하기 쉬운 형태로 정보를 가공해 던져주는 것이 핵심이다.
- **도구 충돌 주의**: 여러 MCP 서버(예: Brave Search 등)를 동시에 켜두면 Agent가 어떤 검색 도구를 쓸지 혼란스러워할 수 있으므로, 웹 검색용으로는 Tavily 하나만 활성화하는 것이 효율적이다.
- **Remote 권장**: 로컬에서 npx로 매번 실행하는 것보다, 원격 URL 방식을 사용하면 초기 구동 지연(Cold Start) 없이 즉각적인 응답을 받을 수 있다.

## 📚 References
- [Tavily AI Official Website](https://tavily.com/)
- [Tavily MCP GitHub Repository](https://github.com/tavily-ai/tavily-mcp)
