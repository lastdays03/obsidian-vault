---
tags: [knowledge/topic, methodology/antigravity]
Up: [[Antigravity_MOC]]
---

# 03. Claude Skills Methodology

**Goal**: 복잡한 코딩 작업을 **"Skill"** 단위로 쪼개서 수행하는 **[[Agentic_Coding|Agentic Coding]]** 방법론을 이해합니다.
**Reference**: `.agent/references/obsi-knowledge-refiner/SKILL.md`

## 1. What is a Skill?
- **정의**: 에이전트가 완수할 수 있는 잘 정의된(Well-defined) 작업 단위.
- **예시**: "기능 기획(Feature Planner)", "코드 리뷰(Reviewer)", "아키텍처 설계".
- **Antigravity 매핑**: Claude의 **Skill** = [[Antigravity_Workflow|Antigravity Workflow]].

## 2. Core Philosophy (철학)
1.  **One Task, One Workflow**: 한 번에 너무 많은 걸 하려 하지 마라.
2.  **Context is King**: 작업을 시작하기 전에 충분한 정보를 줘라 (Context Analysis).
3.  **Human in the Loop**: 중요한 결정 순간에는 반드시 사용자에게 물어봐라 (`notify_user`).

## 3. Conceptual Mapping (개념 매핑)

Claude Code의 개념은 Antigravity의 기능과 다음과 같이 매핑됩니다.

| Claude Code 개념        | Antigravity 대응 기능                    | 설명                                  |
| :---------------------- | :--------------------------------------- | :------------------------------------ |
| **Skill 정의**          | **Workflow** (`.agent/workflows/*.md`)   | 에이전트가 따라야 할 단계별 절차 정의 |
| **Plan 템플릿**         | **Artifacts** (`implementation_plan.md`) | 계획 수립을 위한 전용 아티팩트        |
| **Phases & Checklists** | **Task Boundaries** (`task.md`)          | 진행 상황을 추적하는 살아있는 문서    |
| **User Approval**       | **`notify_user` Tool**                   | 사용자 승인을 대기하는 메커니즘       |
| **Quality Gates**       | **Verification Mode**                    | 검증 단계 및 `walkthrough.md`         |

## 4. Comparison (비교: 기존 vs Agentic)
| 기존 방식                        | Claude Skills (Antigravity)                                        |
| :------------------------------- | :----------------------------------------------------------------- |
| "로그인 기능 만들어줘" (한 방에) | 1. `/feature_planner` (계획) <br> 2. `task_boundary` (단계적 실행) |
| 결과물 예측 불가                 | 단계별 검증으로 고품질 보장                                        |

## 5. 실습 (Action Item)
1. `resources/SKILL.md`를 정독하세요.
2. "자신만의 Skill(워크플로우)"을 하나 상상해 보세요. (예: 버그 수정 워크플로우?)

## 💡 Key Insights
*   **모듈형 문제 해결**: 복잡한 거대 작업을 'Skill(워크플로우)' 단위로 쪼개면, 각 단계를 독립적으로 검증하고 개선할 수 있어 유지보수성이 높아집니다.
*   **인간 개입의 최적화**: 모든 것을 자동화하는 것이 아니라, `notify_user`를 통해 중요한 의사결정 순간에만 인간이 개입함으로써 효율성과 통제권을 모두 확보합니다.
