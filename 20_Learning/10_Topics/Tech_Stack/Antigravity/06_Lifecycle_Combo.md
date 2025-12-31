# 06. Lifecycle Combo (프로젝트 생애주기)

**Goal**: 프로젝트의 시작부터 끝까지를 책임지는 3가지 워크플로우(`Kickoff`, `Planner`, `Archive`)의 연결 흐름을 익힙니다.

## 1. The Pipeline
```mermaid
graph LR
    A[Kickoff] --> B[Feature Planner]
    B --> C(Development...)
    C --> D[Archive]
```

### Step 1: 시작 (Kickoff)
- **명령**: `/project_kickoff`
- **역할**: "깨끗한 책상 준비".
- **준비물**: 프로젝트 이름, 타입(Study/Dev).
- **결과**: `Overview`, `Plan`, `Git` 환경이 세팅됨.

### Step 2: 진행 (Feature Planner)
- **명령**: `/feature_planner`
- **역할**: "지도 보고 운전하기".
- **준비물**: `plan-template.md`.
- **결과**: `implementation_plan.md` (구현 계획서).
- **반복**: 기능 하나 만들 때마다 이 워크플로우를 돌립니다.

### Step 3: 종료 (Archive)
- **명령**: `/archive_project`
- **역할**: "책상 치우고 퇴근하기".
- **준비물**: 완료된 프로젝트.
- **결과**: `90_Archives`로 이동, `20_Learning`으로 지식 이관(Harvesting).

## 2. 시나리오 실습 (Toy Project)
1. `/project_kickoff` -> `Toy_Calculator` 생성.
2. `/feature_planner` -> "덧셈 기능 구현" 계획 수립.
3. `/archive_project` -> 프로젝트 종료 및 삭제.

## 💡 Key Insights
*   **순환 구조**: 프로젝트 생애주기는 직선이 아니라 순환 구조이며, 종료된 프로젝트의 산출물(Archive)은 다음 프로젝트를 위한 비옥한 거름(Knowledge)이 됩니다.
*   **시작의 중요성**: Kickoff 단계에서 구조를 잘 잡아두면, 종료(Archive) 시점에 정리할 시간이 절반으로 줄어듭니다.
