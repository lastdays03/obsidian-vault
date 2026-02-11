# 개념 (Concept): Linear

**태그**: #knowledge/concept #topic/Project_Management #tool/issue-tracker
**출처**: User Input

---

## 📖 정의 (Definition)
*Linear는 소프트웨어 팀을 위해 설계된, 마우스가 필요 없을 정도로 빠르고 주관이 뚜렷한(Opinionated) 이슈 트래커이다.*
기존의 무거운 Jira를 대체하기 위해 탄생했으며, "속도"와 "개발 본연의 업무"에 집중할 수 있도록 돕는다.

---

## 💡 예시 (Example)
*GitHub 커밋 메시지로 이슈 자동 완료:*
```bash
git commit -m "feat: 로그인 API 구현 (Fix T-12)"
# 결과: Linear의 T-12 이슈가 자동으로 'Done' 처리됨
```

---

## 🏗️ 기본 구조 (Hierarchy)
Linear는 **Workspace > Team > Project > Issue** 순서로 구성된다.

1. **Workspace**: 회사 전체 (예: `StepZero Inc.`)
2. **Team**: 기능 조직 (예: `Engineering`, `Design`).
    - *Tip*: 소규모 팀(1인~소수)은 **`Core Team` 하나만** 운영하는 것이 효율적이다. 팀이 나뉘면 이슈 이동이 번거롭다.
3. **Project**: 큰 목표 (예: `MVP 런칭`, `결제 시스템 연동`). 완료 날짜가 있는 큰 업무 단위.
4. **Issue**: 실제 할 일 (예: `로그인 페이지 퍼블리싱`, `API 버그 수정`).

---

## 🔄 GitHub 연동 (Detailed Integration Guide)
Linear와 GitHub의 연동은 개발팀의 생산성을 극대화하는 가장 강력한 기능이다. **"코드(GitHub)와 업무(Linear)의 동기화"**가 핵심이다.

### 1. 핵심 기능 (Core Features)
- **PR & 이슈 자동 연결**: PR 제목이나 브랜치 이름에 이슈 ID(예: `ENG-123`)가 포함되면 자동으로 연결된다.
- **워크플로우 자동화**:
    - **PR 생성 시** → `In Progress`로 자동 변경.
    - **PR 리뷰 요청 시** → `In Review`로 변경 가능.
    - **PR 병합(Merge) 시** → `Done`으로 자동 완료.
- **Git 브랜치 도우미**: Linear에서 '브랜치 이름 복사' 클릭 시 컨벤션에 맞는 이름(`user/eng-123-fix-bug`) 생성.

### 2. 설정 가이드 (Setup Step-by-Step)
Linear 워크스페이스 관리자 권한 필요.
1. **기능 활성화**: `Cmd + K` > "Settings" > "Integrations" > "GitHub" > **Connect**.
2. **리포지토리 선택**: 연동할 Organization 및 Repository 선택 (보안상 필요한 리포지토리만 선택 권장).
3. **자동화 규칙 설정 (Automation)**:
    - **Draft PRs**: `Do not update status`
    - **Open PRs**: `In Progress`
    - **PR Review**: `In Review`
    - **Merge**: `Done`

### 3. 도입 효과 (Benefits)
| 구분         | 효과                       | 상세                                                          |
| :----------- | :------------------------- | :------------------------------------------------------------ |
| **개발자**   | **컨텍스트 스위칭 최소화** | 터미널/GitHub에서 작업만 하면 Linear 상태가 자동 업데이트됨.  |
| **매니저**   | **실시간 진행률 파악**     | 개발자에게 물어볼 필요 없이 PR 상태로 진행 상황 확인 가능.    |
| **히스토리** | **코드-기획 연결**         | 코드의 기획 의도를 알고 싶을 때 Linear 이슈로 바로 이동 가능. |

### 4. Pro Tips
- **매직 워드 불필요**: `Fixes #123` 안 써도 됨. 브랜치 이름에 ID만 있으면 됨.
- **다중 이슈 닫기**: PR 설명에 `Fixes ENG-101, Fixes ENG-102` 쓰면 한 번에 처리 가능.
- **개인 뷰**: GitHub 사이드바에서 내 Linear 이슈 확인 가능.

---

## ⚡ 핵심 워크플로우 (Workflow)

### A. Triage (분류함)
- 새로운 아이디어나 버그 제보가 들어오는 **"임시 보관함"**.
- 바로 Backlog에 넣지 않고, `Triage`에서 처리 방침을 결정한다.
    - **Accept**: 이번 주나 다음 주에 할 일로 등록.
    - **Snooze**: 나중에 다시 확인.
    - **Decline**: 삭제.

### B. Cycle (사이클 = 스프린트)
- 보통 **1주~2주** 단위로 업무를 묶어서 관리한다.
- **자동 이월 (Auto-rollover)**: 기간 내 완료되지 않은 이슈는 **자동으로 다음 사이클로 이동**된다. (죄책감 없는 생산성 철학)

---

## ⌨️ 단축키 (Keyboard First)
마우스 사용을 최소화하여 생산성을 극대화한다.

| 키           | 기능             | 설명                                       |
| :----------- | :--------------- | :----------------------------------------- |
| **C**        | **C**reate       | 새로운 이슈 생성 (어디서든 가능)           |
| **Cmd + K**  | **C**ommand      | 만능 메뉴 (검색, 설정, 뷰 이동 등)         |
| **S**        | **S**tate        | 이슈 상태 변경 (Todo → In Progress → Done) |
| **A**        | **A**ssign       | 담당자 지정                                |
| **X**        | Select           | 이슈 다중 선택                             |
| **G then I** | **G**o **I**nbox | 내 인박스(알림)로 이동                     |

---

## 🎯 추천 시나리오 (Best Practice)
**1인 개발 및 소규모 팀을 위한 루틴 (Weekly Cycle)**

1.  **Planning (월요일)**:
    - 지난주 미완료 이슈는 자동으로 이번 주 Cycle로 넘어옴.
    - `Project`(장기 목표)에서 이번 주에 수행할 이슈를 골라 `Cycle`에 추가.
2.  **Coding (주중)**:
    - Linear 이슈 클릭 → `Cmd + Shift + .` (브랜치명 복사) → `git checkout -b ...`
    - 코드 작업 후 PR 생성 → Linear 이슈 `In Progress` 자동 변경.
    - Merge 시 `Done` 자동 변경.
3.  **Review (금요일)**:
    - `Cmd + K` > "Graph"로 이번 주 속도(Velocity) 확인.
    - 다음 주 업무량 조정.

---

## ⚖️ 비교 (Comparison)
| Feature       | Linear                         | Jira                           |
| :------------ | :----------------------------- | :----------------------------- |
| **철학**      | Opinionated (정해진 방식 따름) | Flexible (모든 것 설정 가능)   |
| **속도**      | 0.1초 반응 (Keyboard First)    | 느림 (로딩, 복잡한 UI)         |
| **대상**      | 스타트업, 메이커, 빠른 실행 팀 | 대기업, 보고서/관리 중심 조직  |
| **기능**      | Cycles (자동 이월), Triage     | Sprints (엄격한 관리), Backlog |
| **초기 설정** | 거의 없음 (바로 시작)          | 복잡함 (워크플로우 정의 필요)  |

---

## 🔑 Key Insights
- **The Linear Method**: 도구가 일하는 방식을 강제하여 팀의 생산성을 높인다.
- **Cycle & Auto-rollover**: 완료하지 못한 일을 자동으로 넘겨주어, 계획 실패에 대한 죄책감보다는 "지속적인 흐름"에 집중하게 한다.
- **Integration**: GitHub와의 연결이 매우 매끄러워, 이슈 트래커와 코드 베이스가 별도로 놀지 않고 하나처럼 움직인다.

---

## 📚 References
- [Productivity for Developers: Linear App Review](https://www.youtube.com/watch?v=F3aJ7W_T33c)
