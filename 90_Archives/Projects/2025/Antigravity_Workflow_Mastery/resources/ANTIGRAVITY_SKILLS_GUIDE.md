# Antigravity Skills 적용 가이드

이 가이드는 Claude Code의 "Skills" 개념(`SKILL.md` 참조)을 Google Antigravity 환경에 맞게 조정하여 사용하는 방법을 설명합니다.

## 개념 매핑 (Conceptual Mapping)

사용자가 제공한 문서에 정의된 "Skill"(복잡한 코딩 작업을 완료하기 위한 구조화된 프로세스)은 **Antigravity Workflows** 및 **Agentic Mode Artifacts 시스템**과 직접적으로 매핑됩니다.

| Claude Code 개념 | Antigravity 대응 기능 | 설명 |
|------------------|------------------------|------|
| **Skill 정의** | **Workflow** (`.agent/workflows/*.md`) | 에이전트가 따라야 할 단계별 절차를 정의합니다. |
| **Plan 템플릿** | **Artifacts** (`implementation_plan.md`) | Antigravity는 계획 수립을 위한 전용 아티팩트를 이미 내장하고 있습니다. |
| **단계(Phases) & 체크리스트** | **Task Boundaries** (`task.md`) | `task.md` 아티팩트는 세부 진행 상황을 추적하는 살아있는 문서 역할을 합니다. |
| **사용자 승인** | **`notify_user` 도구** | `BlockedOnUser: true`를 사용하여 사용자 검토를 위해 작업을 일시 중지합니다. |
| **품질 게이트 (Quality Gates)** | **Verification 모드** | `implementation_plan.md`와 `walkthrough.md`에 명시적인 검증 단계를 문서화합니다. |

## Antigravity에 Skill 이식하는 방법

**Feature Planner**와 같은 Skill을 Antigravity에 "설치"하려면 **Workflow**를 생성해야 합니다.

### 1. Workflow 파일 생성
작업 공간의 `.agent/workflows/feature_planner.md` 경로에 마크다운 파일을 생성합니다. (이미 생성됨)

### 2. 단계 정의 (Define the Steps)
`SKILL.md`의 단계를 에이전트를 위한 자연어 지침으로 변환합니다. 구체적인 도구(Tool) 사용법을 명시하세요.

**변환 예시:**

*   **요구사항 분석** → 지침: "관련 핵심 파일을 읽고 `task.md`를 단계별로 분류하여 업데이트하세요."
*   **계획 수립** → 지침: "TDD 구조에 따라 `implementation_plan.md`를 생성하세요."
*   **품질 게이트** → 지침: "VERIFICATION 모드에서 작업을 완료하기 전에 모든 테스트가 통과하는지 확인하세요."

### 3. Skill 사용
파일이 생성되면 다음과 같이 요청하여 Skill을 실행할 수 있습니다: *"기능 A에 대해 feature planner 워크플로우를 실행해 줘"*

## 가용한 워크플로우 (Available Workflows)

현재 이 프로젝트에서 사용 가능한 워크플로우 목록입니다. `.agent/workflows/` 디렉토리에서 전체 정의를 확인할 수 있습니다.

*   **/archive_project**: 완료된 프로젝트를 검증(Dependency Check) 및 정리(Cleanup) 후 연도별 아카이브로 이동
*   **/concept_distiller**: AI를 활용해 핵심 개념 추출 및 지식 베이스(20_Learning)와 연결
*   **/feature_planner**: 고급 기능 구현을 위한 계획 수립 (TDD, 리스크 평가, 롤백 전략 포함)
*   **/knowledge_harvester**: 프로젝트/인박스의 실전 노트(Topic Note)를 지식 베이스로 이관
*   **/moc_builder**: 노트들을 분석하여 구조화된 MOC(Map of Content) 자동 생성
*   **/project_kickoff**: 표준 구조를 갖춘 새 프로젝트 생성 및 템플릿 적용
*   **/weekly_review**: 주간 회고, 인박스 제로, 액션 아이템 추출 자동화

## 예시: "Feature Planner" 워크플로우

`.agent/workflows/feature_planner.md` 파일을 참조하세요.

```markdown
---
description: TDD 단계와 품질 게이트를 사용하여 기능을 계획하고 구현합니다.
---

# Feature Planner Workflow

이 워크플로우는 엄격한 TDD 주기와 단계별 계획을 사용하여 기능을 구현하도록 안내합니다.
... (이하 생략)
```

## 요약

매번 `plan-template.md`를 수동으로 파싱할 필요가 없습니다. 해당 템플릿을 사용하는 *프로세스*를 **Antigravity Workflow**로 인코딩하면 "Skill"을 자동화할 수 있습니다.
