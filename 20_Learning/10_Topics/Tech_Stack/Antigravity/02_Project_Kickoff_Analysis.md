# 02. Project Kickoff Analysis

**Goal**: `/project_kickoff` 워크플로우가 **표준화된 프로젝트**를 어떻게 보장하는지 분석합니다.

## 1. Why Kickoff Workflow?
- **문제**: 매번 프로젝트 만들 때마다 폴더 만들고, README 쓰고, 태그 다는 게 귀찮고 실수하기 쉽습니다.
- **해결**: 이 과정을 자동화하여 **"시작부터 정리된 상태"**를 만듭니다.

## 2. Key Steps Analysis
### Step 1: Conflict Check (충돌 방지)
- "같은 이름의 프로젝트가 있나?" (`find_by_name`)
- 덮어쓰기 방지를 위한 안전장치(Safety Guard).

### Step 2: Template Selection (유형별 최적화)
- **Study**: 학습에 집중할 수 있게 `Reference`, `Log` 파일 생성.
- **Dev**: 코딩에 집중할 수 있게 `src`, `git init` 제안.

### Step 3: Standard Artifacts (표준 산출물)
- `Overview.md`: 프로젝트의 대시보드.
- `Plan.md`: 해야 할 일 목록.
- `Traces`: 로그 파일 등.

## 3. 실습 (Action Item)
1. 이 프로젝트(`Antigravity_Workflow_Mastery`)의 `Overview.md`를 열어보세요.
2. `/project_kickoff` 워크플로우 파일의 **3단계(Execution)** 정의 부분과 실제 생성된 파일 내용을 비교해 보세요.

## 💡 Key Insights
*   **시작의 표준화**: 모든 프로젝트가 동일한 구조로 시작되면, 새로운 프로젝트를 열 때마다 발생하는 인지 부하(Context Switching Cost)가 획기적으로 줄어듭니다.
*   **안전망 구축**: `Conflict Check`와 같은 안전 장치는 에이전트가 기존 작업을 실수로 덮어쓰는 것을 원천적으로 방지합니다.
