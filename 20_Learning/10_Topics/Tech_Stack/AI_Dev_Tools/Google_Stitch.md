# 개념 (Concept): Google Stitch MCP

**태그**: #knowledge/concept #topic/Tech_Stack/AI_Dev_Tools #tool/ui-generator
**출처**: User Input

---

## 📖 정의 (Definition)
**Google Stitch MCP**는 Google의 AI 디자인 도구인 'Stitch'를 Claude, Cursor, Gemini와 같은 AI 에이전트와 연결해주는 기술이다.
**채팅만으로 UI 디자인을 생성하고, 이를 바로 코드로 변환**하여 프로젝트에 적용할 수 있게 해준다.

---

## 💡 예시 (Example)
*디자인 생성부터 코드 변환까지:*
> 사용자: "여행 앱을 만들고 있어. 파스텔 톤의 **'호텔 예약 상세 화면'**을 디자인해줘."
> (Stitch가 디자인 생성)
> 사용자: "이 디자인을 **Tailwind CSS와 React 코드**로 변환해줘."
> (Stitch MCP가 코드를 생성하여 파일로 저장)

---

## 🛠️ 핵심 기능 (Key Features)

### 1. Design to Code (디자인 → 코드 직행)
Figma나 별도 툴 없이, 채팅창에서 "로그인 화면 만들어줘"라고 하면 디자인 시안과 코드를 동시에 생성한다.

### 2. Design DNA (스타일 상속)
기존 프로젝트의 색상, 폰트, 레이아웃 스타일(Design DNA)을 AI가 인식하여, 이질감 없는 새로운 페이지를 추가할 수 있다.

### 3. No Context Switching
디자인 툴과 에디터를 오갈 필요 없이 개발 환경(IDE) 안에서 디자인과 퍼블리싱을 끝낼 수 있다.

---

## 🚀 설치 및 설정 (Setup Step-by-Step)

### 준비물
- Node.js (v18 이상)
- Google Cloud CLI (gcloud)
- MCP Client (Claude Desktop, Cursor 등)

### Step 1: 자동 설치 (추천)
터미널에서 아래 명령어 실행 (GCP 프로젝트 설정, 인증, 권한 부여 자동 수행):
```bash
npx @_davideast/stitch-mcp init
```

### Step 2: 클라이언트 연결 (Claude Desktop 예시)
`claude_desktop_config.json`에 설정 추가:
```json
{
  "mcpServers": {
    "stitch": {
      "command": "npx",
      "args": [
        "-y",
        "@_davideast/stitch-mcp"
      ],
      "env": {
        "GOOGLE_CLOUD_PROJECT": "생성된-프로젝트-ID" 
      }
    }
  }
}
```

---

## ⚖️ 비교 (Comparison)
| Feature   | Google Stitch MCP               | v0.dev (Vercel)      | Galileo AI           |
| :-------- | :------------------------------ | :------------------- | :------------------- |
| **핵심**  | AI 에이전트(Claude/Cursor) 연동 | 웹 기반 생성 도구    | Figma 플러그인       |
| **Code**  | IDE 내 직접 파일 생성           | 복사/붙여넣기 필요   | Figma 디자인 생성    |
| **Style** | 기존 프로젝트 DNA 상속          | Shadcn/Tailwind 기반 | 텍스트 프롬프트 의존 |

---

## 🔑 Key Insights
- **Agentic Workflow**: 단순히 코드를 짜주는 것을 넘어, '기획(텍스트) → 디자인(Stitch) → 퍼블리싱(Code)'의 전체 흐름을 에이전트 하나로 통합한다.
- **Design DNA**: 기존 프로젝트의 스타일을 분석하고 유지하는 능력 덕분에, 뜬금없는 디자인이 아닌 '우리 앱 같은' 페이지를 만들어준다.
- **1인 개발 최적화**: 디자이너와 퍼블리셔가 없는 1인 개발자나 빠른 프로토타이핑이 필요한 팀에게 강력하다.

---

## 📚 References
- [Google Stitch MCP Package](https://www.npmjs.com/package/@_davideast/stitch-mcp)
