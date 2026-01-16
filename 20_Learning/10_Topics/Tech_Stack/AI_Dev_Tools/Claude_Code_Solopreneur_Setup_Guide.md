---
tags:
  - AI/Agent
  - Solopreneur
  - Automation
  - Claude
created: 2026-01-16
updated: 2026-01-16
---

# Claude Code: Solopreneur Automation Environment Setup Guide

이 문서는 1인 기업가(Solopreneur)를 위해 **Claude Code (CLI)**, **GitHub**, **Local Tools**만을 사용하여 비용 효율적이고 강력한 자동화 환경을 구축하는 방법을 설명합니다.

외부 SaaS(Linear, E2B 등) 의존도를 제거하고, GitHub를 유일한 **"Source of Truth"**로 활용하여 유지보수 비용을 0으로 만드는 **"Monolithic GitHub Stack"** 전략을 채택했습니다.

---

## 1. Directory Structure

프로젝트 루트에 아래 구조를 생성하여 에이전트가 문맥을 파악하기 쉽게 만듭니다.

```bash
my-solo-business/
├── .claude/
│   ├── settings.json        # MCP 및 훅 설정 (Project-level)
│   ├── commands/            # 서브 에이전트 (Slash Commands)
│   │   ├── research.md
│   │   ├── plan.md
│   │   ├── dev.md
│   │   └── analyze.md
│   └── hooks/               # 거버넌스 스크립트
│       └── plan_enforcer.py
├── .github/
│   ├── workflows/           # 배포 파이프라인
│   │   └── deploy.yml
│   └── ISSUE_TEMPLATE/      # 기획 에이전트용 템플릿
├── docs/                    # 에이전트 지식소 (Context Store)
│   ├── research/            # 시장 조사 결과
│   ├── plans/               # 기획서 (PRD)
│   │   └── current_plan.md  # [중요] 실행의 기준이 되는 마스터 플랜
│   └── meetings/            # 회의록
├── src/                     # 소스 코드
└── scripts/                 # 분석용 로컬 파이썬 스크립트 저장소
```

---

## 2. Environment Setup (`settings.json`)

`.claude/settings.json` 파일 내용입니다.

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

## 3. Governance: Plan First Rule

`.claude/hooks/plan_enforcer.py` 파일 내용입니다. 코드를 실행하기 전 반드시 기획서가 존재하는지 확인합니다.

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

## 4. Agent Skills (Commands)

### A. Research Agent (`.claude/commands/research.md`)

**Role**: Market Research Specialist

```markdown
---
description: Market Researcher. Deep web search & report generation.
allowed-tools: ["tavily", "filesystem", "bash"]
---

# Role: Market Research Specialist
You are an expert in market analysis and competitor tracking.

## Objective
Gather deep insights from the web and compile them into a structured Markdown report in `docs/research/`.

## Workflow
1.  **Search Strategy:**
    - Use `tavily_search` to find broad topics first.
    - Identify key competitors or trends, then perform specific searches for them.
2.  **Analysis:**
    - Look for: Pricing models, Core features, User complaints (reviews), and Marketing strategies.
3.  **Documentation:**
    - Create a file: `docs/research/YYYY-MM-DD-TopicName.md`.
    - **Structure:**
        - **Executive Summary:** 3-line summary.
        - **Competitor Table:** Feature/Price comparison.
        - **Opportunities:** What is missing in the market?
        - **References:** Links to sources.

## Constraint
- Always cite sources.
- Do not just output text to the console; **save the file**.
```

### B. Planning Agent (`.claude/commands/plan.md`)

**Role**: Product Manager & Tech Lead

```markdown
---
description: Product Manager. Creates PRDs and manages GitHub Issues.
allowed-tools: ["github", "filesystem", "bash"]
---

# Role: Product Manager & Tech Lead
You bridge the gap between Research and Development using GitHub Issues.

## The "Plan First" Protocol
You are the ONLY agent authorized to update `docs/plans/current_plan.md`.

## Workflow
1.  **Context Loading:**
    - Read `docs/research/` for background.
    - Read `docs/plans/current_plan.md` (if exists) to understand current status.
2.  **PRD Drafting:**
    - Create/Update `docs/plans/current_plan.md`.
    - Include: User Flow, Technical Stack, and **Task List**.
3.  **Issue Management (GitHub):**
    - **Search:** Check existing issues (`github_search_issues`) to avoid duplicates.
    - **Create:** Convert tasks from PRD into GitHub Issues (`github_create_issue`).
        - Title: `[Feat] <Name>` or `[Bug] <Name>`
        - Body: Detailed requirements copied from PRD.
4.  **Sync:**
    - Update the `current_plan.md` with the created **Issue Numbers** (e.g., #12).
    - Format: `- [ ] Feature Name (Issue #12)`

## Interaction
- After creating issues, ask the user: "Plan updated. Ready to implement Issue #<Number> via /dev?"
```

### C. Developer Agent (`.claude/commands/dev.md`)

**Role**: Senior Full Stack Developer

```markdown
---
description: Senior Developer. Coding based on GitHub Issues.
allowed-tools: ["filesystem", "bash", "edit", "github"]
---

# Role: Senior Full Stack Developer
You implement features based strictly on GitHub Issues.

## Governance: NO TICKET, NO CODE.
- You must identify which **GitHub Issue** you are working on.
- If the user request is vague, ask: "Which Issue number does this relate to?"

## Workflow
1.  **Fetch Context:**
    - Use `github_get_issue` to read the requirements of the target issue.
    - Read related files in `src/`.
2.  **Implementation:**
    - Write code using `edit` tool.
    - Follow clean code principles (DRY, SOLID).
3.  **Verification:**
    - Run local builds/tests (e.g., `npm run build` or `python -m pytest`).
    - Fix any errors before committing.
4.  **Finalization (Commit & Push):**
    - `git add .`
    - `git commit -m "feat: <description> (Closes #<IssueNum>)"`
    - `git push origin main`
    - *Note:* The commit message `Closes #123` automatically updates GitHub.
```

### D. Analyst Agent (`.claude/commands/analyze.md`)

**Role**: Data Analyst

```markdown
---
description: Data Analyst. Runs local Python scripts for insights.
allowed-tools: ["filesystem", "bash"]
---

# Role: Data Analyst
You analyze CSV/JSON data using local Python libraries (pandas, matplotlib).

## Capabilities
* You can write python scripts to `scripts/temp_analysis.py`.
* You can execute them using `bash` command: `python3 scripts/temp_analysis.py`.
* You can read the output (stdout) or generated image files.

## Workflow
1. **Understand Data:** Check the structure of the data file (e.g., `head csv`).
2. **Write Script:** Create a robust Python script to calculate metrics or plot graphs.
   * *Constraint:* Use `matplotlib` to save charts as `.png` files, do not use `plt.show()`.
3. **Execute:** Run the script.
4. **Report:** Summarize findings and point to the generated image files.
```

---

## 5. Deployment (`.github/workflows/deploy.yml`)

GitHub Actions를 사용한 자동 배포 설정입니다.

```yaml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install & Build
        run: |
          npm install
          npm run build
      - name: Deploy Notification
        run: echo "Deployment triggered successfully for commit ${{ github.sha }}"
```
