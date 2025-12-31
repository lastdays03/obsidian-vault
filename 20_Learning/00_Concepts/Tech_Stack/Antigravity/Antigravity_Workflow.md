Data Source: [[01_Workflow_Structure]]

# Antigravity Workflow

## Definition
**Antigravity Workflow**는 에이전트(Agent)가 복잡한 작업을 수행하기 위해 따르는 구조화된 실행 절차(Executable Instructions)입니다. 단순한 텍스트 가이드가 아니라, 시스템이 해석하고 실행할 수 있는 "Skill" 단위입니다.

## Structure
- **Frontmatter**: `description`으로 워크플로우의 목적을 정의합니다.
- **Steps**:
    1.  **Analysis**: 현재 상태 파악.
    2.  **Interaction**: 사용자 입력 요청.
    3.  **Execution**: 실제 작업 수행.
    4.  **Verification**: 결과 검증.

## Usage
`.agent/workflows/` 디렉토리에 마크다운(`.md`) 파일로 저장되며, 에이전트는 이를 읽어 해당 워크플로우를 "설치"하고 실행합니다.
