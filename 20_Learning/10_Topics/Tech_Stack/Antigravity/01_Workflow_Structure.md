# 01. Workflow Structure (.agent/workflows)

**Goal**: [[Agentic_Workflow|안티그래비티 에이전트]]가 "어떻게 일하는지" 정의하는 [[Antigravity_Workflow]] 파일의 구조를 이해합니다.

## 1. 파일 위치 및 규칙
- **경로**: `.agent/workflows/`
- **확장자**: `.md` (Markdown)
- **Frontmatter**:
  ```yaml
  ---
  description: 워크플로우에 대한 짧은 요약 (에이전트가 이 설명을 보고 선택함)
  ---
  ```

## 2. 필수 구성 요소
워크플로우는 단순한 텍스트가 아니라 **"실행 가능한 지침서(Executable Instruction)"**입니다.

### A. Title & Definition
- 명확한 제목 (`# Expert Project Kickoff Workflow`)
- 이 워크플로우의 목적과 철학 ("단순 생성이 아닌 검증된 절차...")

### B. Steps (단계)
가장 중요한 부분입니다. 에이전트는 이 단계를 순차적으로 수행합니다.
- **Analysis (분석)**: 시작 전 상태 확인 (`ls`, `grep` 등)
- **Interaction (상호작용)**: 사용자에게 물어볼 질문 정의 (`notify_user`)
- **Execution (실행)**: 실제 파일 생성 및 수정 (`write_to_file`)
- **Verification (검증)**: 결과물이 맞는지 확인

## 3. Workflow Categories (활용 가이드)
목적에 따라 적절한 워크플로우를 선택하세요.

### A. Strategy & Start (시작)
- **/obsi_project_kickoff**: 프로젝트 초기 세팅. 폴더 구조와 기본 문서 자동 생성.
- **/dev_init**: 개발 전용 환경 초기화.

### B. Development (개발)
- **/dev_feature_planner**: 기능 구현. TDD와 엄격한 프로세스.
- **/obsi_archive_project**: 프로젝트 종료 및 정리.
- **/dev_export**: Obsidian으로 산출물 단순 내보내기.

### C. Data Analysis (데이터 분석)
- **/dev_data_analyst**: 데이터 EDA 및 리포팅. OSEMN 방법론.

### D. Learning & Research (학습)
- **/dev_study_planner**: 신기술 학습. "Break & Fix" 방식의 실습 유도.

### E. Knowledge Management (지식)
- **/obsi_knowledge_harvester**: 프로젝트 노트를 지식 베이스로 이관.
- **/obsi_concept_distiller**: 핵심 개념 추출.
- **/obsi_moc_builder**: 목차(MOC) 생성.
- **/obsi_weekly_review**: 주간 회고 및 정리.

## 4. 실습 (Action Item)
1. `.agent/workflows` 폴더를 열어보세요.
2. `project_kickoff.md` (혹은 `obsi_project_kickoff.md`)를 열고 위 구조와 비교해 보세요.

## 💡 Key Insights
*   **인지적 청사진**: 워크플로우 파일은 단순한 스크립트가 아니라, 에이전트의 사고 방식과 행동 패턴을 결정하는 핵심 설계도입니다.
*   **실행 가능한 지침**: 이 파일들이 정교할수록 AI의 실행 착오가 줄어들고 결과물의 품질이 균일해집니다.
