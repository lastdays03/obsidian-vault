---
tags: [knowledge/concept, topic/AI_Dev_Tools]
Source: Monet.Design
---

# 개념 (Concept): MCP Monet Server (monet-mcp)

**태그**: #knowledge/concept #topic/AI_Dev_Tools
**출처**: Monet.Design Official

---

## 📖 정의 (Definition)
*MCP Monet Server (monet-mcp)는 Monet.Design 플랫폼의 공식 원격(Remote) MCP 서버로, AI 코딩 에이전트(Claude Desktop, Cursor 등)가 자연어 검색을 통해 1000개 이상의 React UI 컴포넌트 레지스트리에 접근하고 코드를 자동 삽입할 수 있게 해주는 도구이다.*

---

## 💡 예시 (Example)
*Claude Desktop에서 사용 예시:*
```bash
claude mcp add --transport http monet-mcp \
  https://www.monet.design/api/remote/mcp \
  --header "Authorization: Bearer your-api-key-here"
```

*또는 설정 파일(`claude_desktop_config.json` / `mcp.json`) 직접 추가:*
```json
{
  "mcpServers": {
    "monet-mcp": {
      "serverUrl": "https://www.monet.design/api/remote/mcp",
      "headers": {
        "Authorization": "Bearer your-api-key-here"
      }
    }
  }
}
```

*프롬프트 예시:*
> "monet-mcp 써서 현대적이고 clean한 hero section 찾아줘. 다크 모드 지원되는 거로, 내 프로젝트 색상 토큰 (--primary, --background) 적용해서 코드 넣어줘"

---

## ⚖️ 비교 (Comparison)
| Feature            | Monet MCP               | Mobbin                    | Shadcn/UI          |
| :----------------- | :---------------------- | :------------------------ | :----------------- |
| **Type**           | Remote MCP Server       | Design Reference Site     | Component Library  |
| **AI Integration** | Direct (Code Insertion) | Manual (Visual Reference) | Manual/CLI         |
| **Content**        | 1000+ React Components  | Real App Screenshots      | Core UI Components |
| **Customization**  | AI-driven Styling       | N/A                       | High               |

## 🔑 Key Insights
- **Remote MCP**: 별도의 로컬 설치 없이 API 키만으로 연결 가능한 Hosted MCP 방식이다.
- **Design-to-Code**: 단순한 코드 스니펫 제공을 넘어, AI가 문맥에 맞게 스타일(Tailwind)을 조정하여 삽입한다.
- **Efficiency**: "AI가 디자인 영감을 코드로 바로 구현"하는 워크플로우를 통해 프로토타이핑 속도를 비약적으로 높인다.

## 📚 References
- [Monet.Design Official Site](https://www.monet.design/)
