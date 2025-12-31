# 10. Utilities & Tools

**Goal**: 프로젝트 간 일관된 환경 구성을 도와주는 유틸리티 도구를 이해합니다.
**Reference**: `ANTIGRAVITY_SKILLS_GUIDE.md`

## 1. Agent Settings Sync
여러 프로젝트에서 동일한 에이전트 설정(Workflow, Rules)을 유지하기 위해 동기화 스크립트를 제공합니다.

### init_agent.sh
- **위치**: `claude_skills/scripts/init_agent.sh`
- **목적**: 현재 프로젝트에 `claude_skills`의 공통 설정을 적용합니다.
- **기능**:
    1. `.agent` 폴더를 현재 프로젝트로 **복사(Copy)**
    2. `.agent` 폴더는 Git에 **포함(Commit)**하여 독립적으로 관리

### 사용법
터미널에서 적용 대상 프로젝트의 루트로 이동 후 실행합니다.

```bash
cd ~/dev/workspace/target_project
../claude_skills/scripts/init_agent.sh
```

## 2. 실습 (Action Item)
1. 새로운 빈 폴더를 하나 만듭니다.
2. `init_agent.sh`를 실행하여 `.agent` 링크가 생성되는지 확인합니다.
