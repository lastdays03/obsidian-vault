# Vercel Agent Skills

> [!INFO] 요약
> **Vercel Agent Skills**는 AI 코딩 에이전트의 능력을 확장하기 위해 Vercel Labs에서 제공하는 스킬(Skill) 모음집입니다. [Agent Skills](https://agentskills.io/) 포맷을 따르며, React 최적화, 웹 디자인 검수, Vercel 배포 자동화 등의 기능을 포함하고 있습니다.

- **저장소**: [GitHub - vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)
- **설치**: `npx add-skill vercel-labs/agent-skills`
- **라이선스**: MIT

---

## 1. 주요 스킬 (Available Skills)

이 저장소는 AI 에이전트가 특정 상황에서 참조하거나 실행할 수 있는 "스킬"들을 제공합니다.

### 1) React Best Practices (`react-best-practices`)
Vercel 엔지니어링 팀이 제시하는 React 및 Next.js 성능 최적화 가이드라인입니다. 8개 카테고리에 걸쳐 40개 이상의 규칙을 포함하고 있습니다.

*   **사용 시점**:
    *   새로운 React 컴포넌트나 Next.js 페이지 작성 시
    *   데이터 페칭 로직 구현 시 (Client/Server)
    *   코드 성능 리뷰 및 최적화 시
*   **주요 커버리지**:
    *   Waterfall 제거 (Critical)
    *   번들 사이즈 최적화 (Critical)
    *   서버 사이드 성능 (High)
    *   렌더링 및 Re-render 최적화

### 2) Web Design Guidelines (`web-design-guidelines`)
웹 인터페이스의 접근성, 성능, UX를 검수하기 위한 100개 이상의 규칙 모음입니다.

*   **사용 시점**:
    *   "내 UI 리뷰해줘", "접근성 체크해줘", "UX 감사해줘" 등의 요청 시
*   **주요 커버리지**:
    *   **접근성(Accessibility)**: Semantic HTML, ARIA 라벨, 키보드 조작 등
    *   **UX & 디자인**: Focus 상태, 폼 검증, 애니메이션(Motion), 타이포그래피
    *   **성능**: 이미지 최적화, 레이아웃 스레싱 방지
    *   **인터랙션**: 터치 액션, 다크 모드, i18n

### 3) Vercel Deploy Claimable (`vercel-deploy-claimable`)
대화창에서 즉시 Vercel로 애플리케이션을 배포할 수 있게 해주는 도구입니다. Claude Desktop 등과 연동하여 사용하기 좋습니다.

*   **기능**:
    *   `package.json`을 분석하여 프레임워크 자동 감지 (Next.js, Vite, Astro 등 40+ 지원)
    *   **Preview URL**: 즉시 확인 가능한 라이브 사이트 주소 제공
    *   **Claim URL**: 배포된 프로젝트의 소유권을 사용자 계정으로 가져올 수 있는 링크 제공
*   **사용 예시**: "이 앱 배포해줘", "프로덕션 배포 진행해"

---

## 2. 구조 및 사용법

### 구조
각 스킬은 표준화된 구조를 따릅니다:
*   `SKILL.md`: 에이전트를 위한 지침서 (Instructions)
*   `scripts/`: 자동화를 위한 헬퍼 스크립트 (선택 사항)
*   `references/`: 참고 문서 (선택 사항)

### 사용법
설치 후 에이전트는 관련 작업(예: "React 컴포넌트 최적화 해줘")이 감지되면 자동으로 해당 스킬을 참조하여 답변하거나 작업을 수행합니다.

```bash
# 스킬 추가
npx add-skill vercel-labs/agent-skills
```
