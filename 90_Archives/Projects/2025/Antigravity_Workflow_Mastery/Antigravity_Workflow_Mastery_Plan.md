# Antigravity_Workflow_Mastery Plan

## Goal
`claude_skills` 가이드에 정의된 Antigravity Skills(워크플로우)를 실제 프로젝트 개발에 어떻게 적용하는지 학습하고 체화한다.

## Curriculum

### Phase 1: Workflow Foundation
- [ ] **Workflow Structure**: [[20_Learning/10_Topics/Tech_Stack/Antigravity/01_Workflow_Structure|Workflow Structure]] (.agent/workflows/*.md 파일 구조 이해하기.)
- [ ] **Project Kickoff**: [[20_Learning/10_Topics/Tech_Stack/Antigravity/02_Project_Kickoff_Analysis|Project Kickoff Analysis]] (/project_kickoff가 어떤 단계를 거치는지 확인.)
- [ ] **Claude Skills Analysis**:
  - [[20_Learning/10_Topics/Tech_Stack/Antigravity/03_Claude_Skills_Methodology|Claude Skills Methodology]] (코딩 작업을 "Skill" 단위로 분해하는 철학 이해.)
  - `resources/plan-template.md`: Feature Planner가 따르는 표준 계획 템플릿 분석.

### Phase 2: Workflow Deep Dive (개별 실습)
- [ ] **Feature Planner**: [[20_Learning/10_Topics/Tech_Stack/Antigravity/04_Feature_Planner_Deep_Dive|Feature Planner Deep Dive]] (`/feature_planner`)
  - `plan-template.md`를 사용하여 기능 구현 계획 세우기.
  - TDD 사이클(Red-Green-Refactor)을 시뮬레이션 문서로 작성해보기.
- [ ] **Knowledge Workflows**: [[20_Learning/10_Topics/Tech_Stack/Antigravity/05_Knowledge_Workflows|Knowledge Workflows Guide]]
  - `/knowledge_harvester`: 임시 노트(`Inbox`)를 지식(`Topic`)으로 승격시키기.
  - `/concept_distiller`: 정의가 명확한 용어를 개념(`Concept`)으로 추출하기.
  - `/moc_builder`: 흩어진 노트를 MOC로 묶어보기.

### Phase 3: Workflow Orchestration (조합 실습)
**Goal**: 여러 워크플로우를 연결하여 거대한 프로세스를 관리하는 능력 배양.

#### 🔄 [[20_Learning/10_Topics/Tech_Stack/Antigravity/06_Lifecycle_Combo|Lifecycle Combo]] (프로젝트 생애주기)
> `Project Kickoff` → `Feature Planner` → `Archive Project`
- [ ] **Scenario**: "가상의 토이 프로젝트(예: Python Calculator)를 시작하고, 기능을 개발한 뒤, 완료하여 아카이브하는 전 과정을 수행하라."
  1. `/project_kickoff`로 프로젝트 생성.
  2. `/feature_planner`로 '덧셈 기능' 계획 및 구현(가정).
  3. `/archive_project`로 프로젝트 종료 및 정리.

#### 🧠 [[20_Learning/10_Topics/Tech_Stack/Antigravity/07_Knowledge_Combo|Knowledge Combo]] (지식 선순환)
> `Weekly Review` → `Knowledge Harvester` → `Concept Distiller`
- [ ] **Scenario**: "한 주 동안 쌓인 무질서한 메모를 영구적인 지식 자산으로 정제하라."
  1. `/weekly_review`로 인박스 정리 및 리뷰.
  2. 가치 있는 메모는 `/knowledge_harvester`로 `20_Learning/10_Topics`로 이관.
  3. 이관된 노트에서 핵심 용어는 `/concept_distiller`로 `20_Learning/00_Concepts`로 추출.

#### 🛠️ [[20_Learning/10_Topics/Tech_Stack/Antigravity/08_Maintenance_Combo|Maintenance Combo]] (구조 유지보수)
- [ ] **Scenario**: "대규모 지도를 그리고 정기적으로 점검하라."
  1. `/moc_builder`로 `Tech_Stack` 등의 대규모 MOC 생성.
  2. `/weekly_review` 시 끊어진 MOC 링크 점검.
