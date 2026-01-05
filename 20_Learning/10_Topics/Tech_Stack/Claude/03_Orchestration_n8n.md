---
tags: [knowledge/topic, tool/claude, automation/n8n]
Up: [[Claude_Code_MOC]]
---

# 03. Orchestration with n8n

Claude Code는 단순한 CLI를 넘어, **Headless Mode**를 통해 CI/CD 및 자동화 파이프라인의 핵심 엔진으로 작동할 수 있습니다. **n8n**은 이러한 AI 에이전트와 외부 시스템(Slack, GitHub)을 연결하는 오케스트레이터입니다.

## 📖 Headless Mode & HITL

### Headless Mode
대화형 인터페이스 없이 프롬프트를 실행하고 종료하는 모드입니다.
- **Flags**: `-p "(prompt)"` (One-shot 실행), `--dangerously-skip-permissions` (샌드박스 내 권한 자동 승인)

### HITL (Human-in-the-loop)
AI의 자율성을 보장하되, **최종 승인(Decision)**은 인간이 담당하는 **[[Human-in-the-loop|HITL (Human-in-the-loop)]]** 디자인 패턴입니다. "AI가 제안하고, 인간이 결재합니다."

## 💻 n8n 워크플로우 구현

### 1. Headless Execution Script
n8n의 `Execute Command` 노드에서 실행할 스크립트입니다.

```bash
#!/bin/bash
PROJECT_DIR="/home/node/workspace/my-project"
PR_DESC=$1

cd "$PROJECT_DIR" || exit 1

# 샌드박스 모드에서 안전하게 실행
claude --dangerously-skip-permissions -p "
Read the PR description from $PR_DESC.
Analyze the code changes against our security guidelines defined in CLAUDE.md.
Output a JSON summary of risks: { \"riskLevel\": \"high|medium|low\", \"summary\": \"...\" }.
Do not output conversational text.
"
```

### 2. Slack Approval Flow (HITL)
1.  **Trigger**: GitHub PR Created.
2.  **AI Action**: Claude가 코드를 분석하여 리스크 리포트 생성.
3.  **Human Request**: Slack으로 "승인(Approve)" / "거절(Reject)" 버튼 전송.
4.  **Suspension**: n8n 워크플로우는 `Wait for Webhook` 노드에서 인간의 클릭을 실시간 대기.
5.  **Resume**: 버튼 클릭 시 워크플로우 재개 -> GitHub Merge 또는 Comment 수행.

## 💡 Key Insights
- **Asynchronous Approval**: AI 작업은 빠르지만, 인간의 검토는 느리다. n8n의 `Wait` 노드는 이 속도 차이를 우아하게 해결한다.
- **Headless Agency**: AI는 항상 대화할 필요가 없다. 백그라운드에서 조용히 일하고 결과(JSON)만 던져주는 것이 더 효율적일 때가 많다.
- **Safety Harness**: HITL은 AI 환각에 대비한 최후의 안전장치다.
