---
tags: [knowledge/topic, methodology/antigravity]
Up: [[Antigravity_MOC]]
---

# 04. Feature Planner Deep Dive

**Goal**: 고급 기능 구현을 위한 계획 수립 도구인 Feature Planner를 마스터합니다.
**Reference**: [[../resources/plan-template|plan-template.md]]

## 1. Feature Planner란?
단순히 "구현해"라고 하는 대신, **"어떻게, 왜, 안전하게"** 구현할지 미리 설계하는 워크플로우입니다.
- **[[TDD]] (Test Driven Development)**: 테스트를 먼저 생각함.
- **Risk Assessment**: 뭐가 잘못될 수 있는지 미리 고민.
- **Rollback Strategy**: 망했을 때 어떻게 되돌릴지 계획.

## 2. Plan Template 구조 분석
`resources/plan-template.md`는 아래 섹션으로 구성됩니다.

### A. User Review Required (주의사항)
- Breaking Changes 등 사용자가 꼭 알아야 할 내용.
- **Why**: 나중에 "어 이거 왜 바꼈어?" 하는 사태 방지.

### B. Proposed Changes (변경 사항)
- **File-by-File**: 어느 파일이 어떻게 바뀌는지 명시.
- **Dependencies First**: 의존성이 있는 파일(DB 스키마 등)부터 나열.

### C. Verification Plan (검증 계획)
- **Automated**: `npm test` 등 명령어.
- **Manual**: "브라우저 열고 로그인 버튼 클릭" 등.

## 3. TDD Simulation (실습)
**가정**: "계산기 앱에 나눗셈 기능 추가"
1. **Red (실패하는 테스트)**: `test_divide_by_zero` 작성.
2. **Green (구현)**: `if b == 0: raise Error` 구현.
3. **Refactor**: 코드 정리.

이 과정을 `/feature_planner`를 실행하여 `implementation_plan.md`에 작성해보는 것이 목표입니다.

## 💡 Key Insights
*   **엔지니어링으로의 격상**: Feature Planner는 단순 코딩을 '리스크 평가'와 '검증 계획'이 포함된 체계적인 엔지니어링 프로세스로 변화시킵니다.
*   **실패 예방**: 코드를 작성하기 전에 실패 가능성(Risk)을 미리 고민하는 것만으로도 버그 발생률을 현저히 낮출 수 있습니다.
