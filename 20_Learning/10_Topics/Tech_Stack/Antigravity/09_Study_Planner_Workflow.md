# 09. Study Planner Workflow

**Goal**: "코드를 고장내며 배우는" 실전형 학습 워크플로우인 **/dev_study_planner**를 이해하고 활용합니다.
**Reference**: `.agent/workflows/study_planner.md`

## 1. 개요 (Overview)

기존의 `Feature Planner`가 "안전하고 견고한 기능 구현"을 목표로 한다면, **Study Planner**는 **"실패를 통한 학습(Learning by Failure)"**을 목표로 합니다. 단순히 문서를 읽는 것이 아니라, 계획을 세우고, 직접 구현하고, 일부러 고장내보는 과정을 통해 깊이 있는 이해를 돕습니다.

## 2. 학습 루프 (The Learning Loop)

이 워크플로우는 **"Plan -> Theory -> Practice -> Verify -> Archive"**의 5단계 사이클을 따릅니다.

### Phase 1: Goal Setting & Planning (계획)
- **Topic Selection**: 무엇을 공부할지 명확히 정합니다. (예: "Python Generator")
- **Study Plan**: `docs/plans/STUDY_제목.md` 파일을 생성하여 커리큘럼을 짭니다.
    - **Deliverables**: 단순 노트가 아닌, '구현 코드'나 '미니 프로젝트'를 목표로 합니다.
    - **Quiz**: 스스로 답해야 할 질문 리스트를 미리 만듭니다.

### Phase 2: Theory (이론)
- 30분 이내로 핵심 개념을 파악합니다.
- **Feynman Technique**: 개념을 3줄로 요약하여 설명할 수 있어야 합니다.

### Phase 3: Practice (실습 - 핵심!)
이 단계가 Study Planner의 꽃입니다.
1.  **Copy & Run**: 예제 코드를 따라 칩니다.
2.  **Tweak & Break (고장내기)**:
    - "이 변수를 지우면 어떻게 될까?"
    - "입력값을 음수로 넣으면?"
    - **일부러 에러를 발생시키고, 로그를 읽고, 다시 고치는 과정**에서 진짜 학습이 일어납니다.

### Phase 4: Experiment & Verify (검증)
- **Scenario Testing**: 계획 단계에서 세운 가설들을 실험합니다.
- **Quiz Solving**: 스스로 낸 퀴즈에 답을 작성합니다.

### Phase 5: Retrospective (회고)
- 오늘의 학습을 3줄로 요약합니다 (`Key Takeaways`).
- 발생했던 에러와 해결책을 기록하여 자산화합니다.

## 3. Feature Planner vs Study Planner

| 특징          | Feature Planner           | Study Planner                      |
| :------------ | :------------------------ | :--------------------------------- |
| **목적**      | 배포 가능한 기능 구현     | 개념 이해 및 체득                  |
| **실행 방식** | TDD, 실패하지 않도록 주의 | **일부러 실패(Break)** 해보며 실험 |
| **결과물**    | Production Code           | Study Notes, Snippets, Q&A         |
| **모드**      | Execution 위주            | Learning Loop 반복                 |

## 4. 실습 (Action Item)
1. `/dev_study_planner`를 실행해 봅니다.
2. 관심 있는 작은 주제(예: `Python 데코레이터`)를 선정합니다.
3. 워크플로우가 안내하는 대로 계획을 세우고, 코드를 고장내보며 학습을 완료해보세요.
