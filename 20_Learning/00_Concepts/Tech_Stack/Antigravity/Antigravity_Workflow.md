---
tags: [knowledge/concept, methodology/antigravity]
Up: [[Antigravity_MOC]]
---

# 개념 (Concept): Antigravity Workflow

**출처**: [[01_Workflow_Structure]]

---

## 📖 정의 (Definition)
**Antigravity Workflow**는 에이전트(Agent)가 복잡한 작업을 수행하기 위해 따르는 구조화된 실행 절차(Executable Instructions)입니다. 단순한 텍스트 가이드가 아니라, 시스템이 해석하고 실행할 수 있는 "Skill" 단위입니다.

---

## 💡 구성 (Structure)
- **Frontmatter**: `description`으로 워크플로우의 목적을 정의합니다.
- **Steps**:
    1.  **Analysis**: 현재 상태 파악.
    2.  **Interaction**: 사용자 입력 요청.
    3.  **Execution**: 실제 작업 수행.
    4.  **Verification**: 결과 검증.

## 💡 Key Insights
- **Agent-Readable**: 인간이 읽기 위한 문서가 아니라, 에이전트가 단계를 밟아가며 실행할 수 있도록 명확한 명령조로 작성되어야 합니다.
- **Modularity**: 하나의 거대한 워크플로우보다는 `Harvest`, `Distill`처럼 기능별로 쪼개어 조합(Compose)할 수 있도록 설계합니다.
