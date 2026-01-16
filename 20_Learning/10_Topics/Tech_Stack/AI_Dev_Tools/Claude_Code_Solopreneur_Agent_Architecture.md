---
tags:
  - AI/Agent
  - Solopreneur
  - Architecture
  - Claude
  - MCP
created: 2026-01-16
updated: 2026-01-16
---

# Claude Code: Solopreneur Agent Architecture

## 1. Introduction: AI Agent Architecture for Solopreneurs

### 1.1 The Paradigm Shift
현대 비즈니스 생태계에서 1인 기업가(Solopreneur)는 기획, 개발, 마케팅, 운영이라는 4대 기둥을 홀로 지탱해야 하는 'Full-Cycle' 책무를 집니다. SaaS 도구의 파편화로 인한 '컨텍스트 스위칭 비용'을 해결하기 위해, 우리는 **Claude Code (CLI)**와 **MCP (Model Context Protocol)**를 활용한 새로운 패러다임을 제시합니다.

이제 사용자는 도구를 직접 조작하는 단계에서 벗어나, 자율 에이전트를 지휘하는 **'System Orchestrator'**로 변모합니다.

### 1.2 Objective & Scope
본 문서는 1인 기업가의 비즈니스 라이프사이클 전반을 자동화하기 위한 기술적 청사진을 제공합니다. 핵심 목표는 **"Plan First(계획 우선)"** 거버넌스 정책이 시스템 레벨에서 강제되는 고도로 통제된 개발 환경을 구축하는 것입니다. 외부 SaaS 의존도를 제거하고 **GitHub**를 유일한 **"Source of Truth"**로 활용하는 **"Monolithic GitHub Stack"** 전략을 채택합니다.

---

## 2. Core Tech Stack & Architecture

### 2.1 System Architecture Diagram (Hub and Spoke)

우리가 구축할 시스템은 Claude Code CLI를 중앙 엔진으로 하는 **Hub and Spoke** 모델입니다.

```mermaid
graph TD
    %% Core Orchestrator
    User[System Orchestrator\n(User)] -->|Commands /plan, /dev| Hub
    
    subgraph Hub [Central Intelligence]
        Claude[Claude Code CLI\n(Orchestrator)]
        Hook[Governance Hooks\n(Plan Enforcer)]
    end

    %% Governance Logic
    Claude -->|Pre-Tool Check| Hook
    Hook -- Allow/Deny --> Claude

    %% Spokes (MCP Servers)
    subgraph Spokes [MCP Agent Skills]
        Research[Research Node\n(Tavily MCP)]
        Plan[Planning Node\n(GitHub MCP)]
        Dev[Dev Node\n(Filesystem & GitHub MCP)]
        Analyst[Analyst Node\n(Local Python)]
    end

    %% Connections
    Claude <-->|MCP| Research
    Claude <-->|MCP| Plan
    Claude <-->|MCP| Dev
    Claude <-->|Shell| Analyst

    %% Data Flow
    Research -->|Market Data| Docs[(Context Store\n/docs)]
    Plan -->|Issues & PRs| GithubRemote[(GitHub Repository)]
    Dev -->|Source Code| Src[(Source Code\n/src)]
```

### 2.2 Component Roles

| Component         | Role                              | Tool / Tech            |
| :---------------- | :-------------------------------- | :--------------------- |
| **Orchestrator**  | 중앙 제어 및 의사결정             | Claude Code CLI        |
| **Governance**    | 실행 통제 및 정책 강제 (Security) | Custom Hooks (Python)  |
| **Research Node** | 시장 조사 및 검증                 | Tavily MCP             |
| **Planning Node** | 기획 및 프로젝트 관리             | GitHub MCP (Issues)    |
| **Dev Node**      | 코드 작성 및 버전 관리            | Filesystem, GitHub MCP |
| **Analyst Node**  | 데이터 분석                       | Local Python Scripts   |

---

## 3. Development Environment Setup

### 3.1 Directory Structure strategy

단일 레포지토리(Monolithic Repository) 전략을 사용하여 에이전트가 전체 맥락을 파악하기 쉽게 구성합니다.

```bash
my-solo-business/
├── .claude/                 # Agent Configuration
│   ├── CLAUDE.md            # System Constitution (Prompt Injection)
│   ├── settings.json        # MCP & Hook Config
│   ├── commands/            # Sub-Agent Prompts (.md)
│   │   ├── research.md
│   │   ├── plan.md
│   │   ├── dev.md
│   │   └── analyze.md
│   └── hooks/               # Governance Scripts
│       └── plan_enforcer.py
├── .github/                 # GitHub Pipelines & Templates
│   ├── workflows/           # Deploy Actions
│   └── ISSUE_TEMPLATE/      # PRD/Bug Templates
├── docs/                    # Global Context Store
│   ├── research/            # Market Analysis
│   ├── plans/               # PRDs & Roadmaps
│   │   └── current_plan.md  # Master Plan (Golden Source)
│   └── meetings/            # 회의록
├── src/                     # Application Source Code
└── scripts/                 # Local Python Analysis Scripts
```

### 3.2 `settings.json` Configuration

GitHub 및 Tavily MCP 서버를 구성합니다.

```json
{
  "mcpServers": {
    "tavily": {
      "command": "npx",
      "args": ["-y", "@mcptools/mcp-tavily"],
      "env": { "TAVILY_API_KEY": "${TAVILY_API_KEY}" }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_PAT}"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]
    }
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write|Bash",
        "command": "python3 .claude/hooks/plan_enforcer.py"
      }
    ]
  },
  "globalTools": ["bash"]
}
```

---

## 4. Governance: "Plan First" Protocol

시스템 레벨에서 **"계획 없는 실행"**을 원천 차단합니다. LLM의 환각이나 일탈을 방지하는 안전장치입니다.

### 4.1 Plan Enforcer Logic (`plan_enforcer.py`)

```python
import sys
import os

def check_plan_exists():
    plan_file = "docs/plans/current_plan.md"
    if not os.path.exists(plan_file):
        print("⛔ Action Blocked: 'current_plan.md' not found.")
        print("Reason: You must create a plan before executing any code.")
        print("Action: Run '/plan' command first to generate the PRD.")
        sys.exit(1)

if __name__ == "__main__":
    check_plan_exists()
    sys.exit(0)
```

---

## 5. Sub-Agent Implementations

각 에이전트의 역할과 책임을 `commands/` 디렉토리 내의 마크다운 파일로 정의합니다.

### 5.1 Research Agent (`/research`)
- **Focus**: Market Validation, Competitor Analysis
- **Tools**: Tavily (Search), Filesystem (Report)
- **Output**: `docs/research/` Reports

### 5.2 Planning Agent (`/plan`)
- **Focus**: PRD Creation, GitHub Issue Management
- **Tools**: GitHub (Issues), Filesystem
- **Workflow**:
    1. Research 문서 리딩.
    2. PRD (`current_plan.md`) 작성.
    3. **GitHub Issue** 생성: `[Feat] <Name>`
    4. PRD에 Issue Number 연동.

### 5.3 Dev Agent (`/dev`)
- **Focus**: Implementation, Verification, Commit
- **Tools**: Filesystem, Bash, GitHub
- **Constraint**: **NO TICKET, NO CODE** (작업 전 Issue 번호 확인 필수)
- **Git Flow**:
    - `Closes #IssueNum` 메시지를 사용하여 커밋 시 자동으로 이슈를 닫습니다.

### 5.4 Analyst Agent (`/analyze`)
- **Focus**: Data Analysis via Local Python
- **Tools**: Filesystem, Bash (python3)
- **Environment**: 로컬 `scripts/` 디렉토리의 Python 스크립트를 생성하고 실행하여 데이터 분석을 수행합니다.

---

## 6. Walkthrough: Idea to Ticket

실제 비즈니스 아이디어가 시스템을 통과하는 흐름입니다.

### Step 1: Market Validation
User invokes `/research "AI quote calculator for developers"`.
- **Agent Action**: Tavily를 사용하여 시장 조사를 수행하고 리포트를 생성합니다.

### Step 2: Governance Check (The Wall)
User tries `/dev "Create Next.js app"`.
- **System Action**: `plan_enforcer.py` 훅이 작동하여 `current_plan.md` 부재를 감지하고 **거부(Deny)**합니다.

### Step 3: Planning & Ticket Generation
User invokes `/plan "Based on research, plan MVP"`.
- **Agent Action**:
    1. 리서치 문서를 읽고 PRD 작성.
    2. **GitHub Issue** 생성 (Issue #1: Setup, Issue #2: Core Logic).
    3. `current_plan.md`에 이슈 번호 명시.

### Step 4: Execution
User invokes `/dev "Start Issue #1"`.
- **System Action**: 계획 파일이 존재하므로 승인.
- **Agent Action**: 코드를 작성하고 테스트 후 `git commit -m "feat: setup project (Closes #1)"`을 실행하여 GitHub Issue를 완료 처리합니다.

---

## 7. Conclusion

이 아키텍처는 기술적인 도구 모음을 넘어선 **'운영 체제(OS)'**입니다. 1인 기업가는 이 시스템을 통해 인지적 부하를 최소화하고, GitHub 기반의 견고한 워크플로우를 통해 아이디어를 현실로 빠르게 구체화할 수 있습니다.
