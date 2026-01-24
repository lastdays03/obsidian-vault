---
type: "[[Guide]]"
tags:
  - "#AI/DevTools"
  - "#NotebookLM"
  - "#AI_Agnecy"
  - "#Antigravity"
  - "#ClaudeCode"
  - "#Cursor"
  - "#NoCode"
  - "#AppDev"
status: "#Done"
created: 2026-01-24
aliases:
  - "NotebookLM AI Agent Workflow Guide"
---

# NotebookLM + AI Agent 앱 만들기 가이드

> [!SUMMARY]
> NotebookLM으로 기획하고, AI 에이전트(Antigravity, Claude Code, Cursor 등)가 구현하는 'AI Agency' 워크플로우 실전 가이드입니다.

## 1. 개요 및 워크플로우

비개발자도 **"기획(Brain) + 구현(Hand)"**의 분업 구조를 통해 고품질 웹 앱을 만들 수 있습니다.

*   **Brain (기획/설계)**: **NotebookLM**
    *   방대한 자료 리서치, 아이디어 구체화, 상세 기획서 작성.
    *   개발자를 위한 "테크니컬 프롬프트" 생성.
*   **Hand (구현/코딩)**: **AI Coding Agents**
    *   **Antigravity** (Google): 폴더 단위 프로젝트 생성 및 풀스택 구현 강점.
    *   **Claude Code** (Anthropic): CLI 기반의 강력한 코딩 에이전트.
    *   **Cursor** (IDE): VS Code 기반의 편집 및 수정에 최적화된 에디터.
    *   **Gemini CLI** (Google): 터미널 환경에서의 빠른 프로토타이핑.

### 전체 프로세스

1.  **Research**: NotebookLM으로 아이디어 관련 소스 수집 및 학습.
2.  **Plan**: 서비스 컨셉, 기능, UX, 데이터 구조 기획.
3.  **Prompting**: AI 에이전트에게 주입할 **"Technical Prompt"** 생성.
4.  **Execution**: 선택한 AI 에이전트(Antigravity/Claude/Cursor)에 프롬프트 입력 및 앱 생성.
5.  **Refinement**: 결과물 확인 후 디테일 수정 및 마케팅 전략 수립.

---

## 2. [Brain] NotebookLM으로 리서치 및 기획

### 2.1 자료 수집 (Source)
만들고자 하는 서비스와 관련된 자료(블로그, 뉴스, 논문, 유튜브 영상 등)를 수집하여 NotebookLM 소스로 추가합니다.
*   **Fast Research**: 핵심 자료 10개 내외.
*   **Deep Research**: 다양한 관점의 자료 30개 이상.

#### 📌 [Prompt] 자료 수집(검색) 요청
> **목표**: 서비스 구축에 필요한 고품질의 레퍼런스 자료를 수집합니다.

```markdown
[Topic]: [서비스명/주제] (예: 개인 맞춤형 식단 관리 웹 앱)

[Request]
위 주제와 관련하여 심도 있는 정보를 제공하는 웹 소스, 기사, 혹은 기술 문서를 찾아주세요.
다음 관점의 자료들이 포함되어야 합니다:

1. **Technical**: 유사한 서비스를 구현하기 위한 최신 기술 스택 및 아키텍처 추천.
2. **UX/UI**: 최신 디자인 트렌드 및 사용자 경험 우수 사례(Case Study).
3. **Market**: 시장 현황, 경쟁사 분석, 비즈니스 모델 예시.
4. **Tutorial**: 실제 개발 과정이 담긴 튜토리얼이나 가이드.
```

### 2.2 기획 구체화 (Chat)
NotebookLM과 대화하며 단순한 아이디어를 **구체적인 기획서(PRD)**로 발전시킵니다.

#### 📌 [Prompt] PRD 생성 요청
> **목표**: 아이디어를 구체적인 제품 요구사항 정의서(PRD)로 변환합니다.

```markdown
Role: Senior Product Manager (PM)

Goal:
제가 구상 중인 [서비스명]에 대한 상세한 제품 요구사항 정의서(PRD)를 작성해 주세요.
이 서비스는 [타겟 사용자]를 대상으로 하며, [핵심 가치/목표]를 제공하는 것이 목적입니다.

Output Format:
1. 제품 개요 (Product Overview)
   - 서비스 한 줄 요약
   - 해결하려는 문제 (Problem Statement)
   - 타겟 사용자 페르소나 (Persona)
2. 핵심 기능 명세 (Core Features)
   - MVP(최소 기능 제품) 단계에서 반드시 필요한 기능 3~5가지
   - 각 기능별 사용자 스토리 (User Story)
3. 사용자 경험 흐름 (User Journey Map)
   - 앱 실행부터 핵심 기능 사용 완료까지의 단계별 시나리오
4. 데이터 구조 (Data Structure Idea)
   - 사용자, 콘텐츠 등 핵심 데이터 엔티티
5. 수익화 모델 (Monetization Strategy)
```

---

## 3. [Bridge] Technical Prompt 생성

기획이 완료되면, AI 에이전트가 이해할 수 있는 **개발자용 상세 지시문(Technical Prompt)**을 만듭니다. 이 단계가 가장 중요합니다.

### 3.1 NotebookLM에게 요청하기
NotebookLM에서 완성된 기획 내용을 바탕으로, 코딩 에이전트에게 전달할 "설계도"를 만듭니다.

#### 📌 [Prompt] AI 코딩 에이전트용 마스터 프롬프트 생성
> **목표**: AI 개발자가 코드를 작성하기 위한 완벽한 지시문을 생성합니다.

```markdown
Role: Senior Tech Lead & Prompt Engineer

Context:
우리는 앞서 논의한 [서비스명] PRD를 바탕으로 실제 웹 애플리케이션을 구축하려고 합니다.
구글의 Antigravity, Anthropic의 Claude Code, 혹은 Cursor와 같은 AI 코딩 에이전트를 사용할 것입니다.

Task:
AI 코딩 에이전트에게 입력할 **"Master Technical Prompt"**를 작성해 주세요.
이 프롬프트는 에이전트가 중단 없이 전체 프로젝트 구조를 잡고 코드를 작성할 수 있도록 매우 구체적이어야 합니다.

Implementation Details (Constraint):
- Frontend: React (Vite), TailwindCSS
- Backend: Node.js (Express) or Next.js (Fullstack)
- Database: Supabase or Local JSON (프로토타입용)
- Language: TypeScript 선호 (혹은 JavaScript)
- Design: Modern, Clean, Responsive UI

Output Structure for the Prompt:
1. **Role Definition**: "You are an expert Full Stack Developer..."
2. **Project Goal**: 앱의 목적과 핵심 가치 요약
3. **Tech Stack**: 사용할 프레임워크 및 라이브러리 명시
4. **File Structure**: 제안하는 폴더 및 파일 구조 (ASCII Tree 형태)
5. **Step-by-Step Implementation Plan**:
   - Step 1: 프로젝트 셋업
   - Step 2: 페이지 및 컴포넌트 생성
   - Step 3: 비즈니스 로직 구현
   - Step 4: 스타일링 및 UI 폴리싱
6. **Detailed Feature Requirements**: 각 기능별 로직 상세 설명
7. **Code Style Guide**: "Use functional components", "Use generic naming" 등
```

### 3.2 프롬프트 검토
NotebookLM이 생성해준 텍스트를 그대로 복사하지 말고, **기술 스택(React, DB 등)이 내가 원하는 것과 맞는지 확인**하고 수정하세요.

---

## 4. [Hand] AI 에이전트로 구현하기

생성된 **Technical Prompt**를 사용하여 선호하는 도구에서 개발을 시작합니다.

### 옵션 A: Antigravity (Google)
*   **특징**: 프로젝트 전체 구조 생성 및 풀스택 구현 강점. "폴더" 개념을 잘 이해함.
*   **실행**:
    1.  Antigravity 실행 -> `Open Folder`.
    2.  프롬프트 입력창에 위에서 만든 **Master Technical Prompt** 붙여넣기.
    3.  `Generate` 실행.

### 옵션 B: Claude Code / Gemini CLI
*   **특징**: 터미널 기반. 속도가 빠르고 파일 생성/수정을 직접 커맨드로 수행.
*   **실행**:
    1.  터미널: `mkdir my-app && cd my-app`
    2.  `claude` 실행.
    3.  **Master Technical Prompt** 붙여넣기.
    4.  에이전트가 제안하는 파일 생성/패키지 설치 등의 액션을 승인(Enter).

### 옵션 C: Cursor (IDE)
*   **특징**: VS Code 기반. UI를 보면서 개발, 강력한 Composer 기능.
*   **실행**:
    1.  Cursor에서 빈 폴더 열기.
    2.  `Cmd+I` (Composer) or `Cmd+L` (Chat) 열기.
    3.  **Master Technical Prompt** 붙여넣기 -> `Create All`.

---

## 5. 수정 및 완성 (Refinement)

앱이 생성되면 실행해보고(`npm run dev`), 디테일을 잡아나갑니다.

### 5.1 에러 수정 (Debugging)
실행 중 에러가 발생하면 에러 로그를 그대로 복사해서 에이전트에게 줍니다.

#### 📌 [Prompt] 에러 디버깅 요청
```markdown
[Error Log]
(터미널이나 브라우저 콘솔의 빨간색 에러 메시지를 붙여넣으세요)

[Request]
위 에러가 발생했습니다. 원인을 분석하고, 수정된 코드를 전체 파일 형식으로 제공해 주세요.
수정할 때는 기존 기능을 유지하면서 에러만 해결해야 합니다.
```

### 5.2 디자인 및 기능 개선 (Polishing)
초기 결과물이 밋밋하다면 디자인을 개선합니다.

#### 📌 [Prompt] UI/UX 개선 요청
```markdown
현재 UI가 너무 기본적입니다. 다음 사항을 반영하여 디자인을 개선해 주세요:

1. [특정 페이지/컴포넌트]에 카드형 레이아웃과 그림자 효과(Shadow)를 추가해 주세요.
2. 버튼에 마우스 호버(Hover) 시 색상이 부드럽게 변하는 애니메이션을 넣어 주세요.
3. 전체적인 색상 테마를 [Blue & White] 톤으로 통일하고, 폰트는 가독성 좋은 산세리프체로 적용해 주세요.
4. 모든 텍스트는 **한국어**로 변경해 주세요.
```

### 5.3 마케팅 전략 수립 (NotebookLM 회귀)
앱이 완성되면 다시 **NotebookLM**으로 돌아가 마케팅 전략을 짭니다.

#### 📌 [Prompt] 마케팅 전략 요청
```markdown
완성된 [앱 이름]의 런칭을 준비하고 있습니다.
초기 사용자 100명을 확보하기 위한 구체적인 GTM(Go-To-Market) 전략을 세워 주세요.

1. **SEO 전략**: 검색되기 좋은 키워드 10개와 블로그 포스팅 주제 5가지.
2. **Social Media**: 인스타그램/트위터에 올릴 홍보 문구와 해시태그.
3. **Community**: 이 앱을 좋아할 만한 온라인 커뮤니티 리스트와 홍보 매너.
```

---

## 6. 실전 팁

1.  **반복(Iteration)**: 한 번에 완벽한 앱은 나오지 않습니다. Technical Prompt를 조금씩 수정해가며 에이전트를 다시 실행해보세요.
2.  **분할 정복**: 앱이 크다면 전체를 한 번에 만들지 말고, "로그인 기능 먼저", "메인 대시보드 먼저"와 같이 기능을 쪼개서 에이전트에게 요청하세요.
3.  **에이전트 조합**: **Antigravity**로 초안을 잡고, **Cursor**로 디테일을 깎는 식의 조합이 매우 효율적입니다.
