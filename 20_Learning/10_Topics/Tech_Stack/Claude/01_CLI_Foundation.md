---
tags: [knowledge/topic, tool/claude]
Up: [[Claude_Code_MOC]]
---

# 01. CLI First Foundation

**CLI-First Foundation**은 Claude Code를 단순한 터미널 도구가 아닌, 로컬 개발 환경과 LLM을 연결하는 '지능형 런타임'으로 활용하기 위한 기반 지식입니다.

## 📖 기술 개요 (Technical Overview)

### React-in-Terminal 아키텍처
Claude Code는 **[[React-in-Terminal]]** 아키텍처(`ink`, `yoga-layout`)를 기반으로 터미널 내에서 가상 DOM(Virtual DOM)을 렌더링합니다. 이는 명령형(Imperative) CLI와 달리, 상태(State) 기반의 선언적 UI를 제공하여 복잡한 스트리밍 중에도 높은 반응성을 보장합니다.

### Apple Silicon (ARM64) 최적화
Rosetta 2 에뮬레이션은 LLM 추론 및 대규모 컨텍스트 처리 시 레이턴시를 유발합니다. 반드시 **네이티브 ARM64 바이너리** 실행을 보장해야 합니다.

### Context Management
`CLAUDE.md`는 프로젝트의 "Long-term Memory"입니다. 단순한 파일이 아니라, AI에게 아키텍처 원칙과 컨벤션을 주입하는 **영속적 컨텍스트(Persistent Context)** 역할을 합니다.

## 💡 Thinking Mode 전략

Claude 3.7+ 모델의 **[[Thinking_Mode|Thinking Mode]]** (추론) 능력을 제어하는 전략입니다.

| 모드 | 명령어 | 토큰 예산 | 적용 시니리오 |
| :--- | :--- | :--- | :--- |
| **Basic** | (Default) | 0 ~ 1k | 단순 구현, 주석, 오타 수정 |
| **Thinking** | `/think` | 2k ~ 8k | 함수 리팩토링, 버그 분석 |
| **Deep Thinking** | `/think hard` | 8k ~ 16k | 아키텍처 설계, 비즈니스 로직 |
| **Ultra Thinking** | `/ultrathink` | 16k ~ 32k | 레거시 역공학, 대규모 마이그레이션 |

## 💻 설정 및 검증 (Setup & Verify)

### Architecture Check Script
```bash
#!/bin/bash
# Verify Apple Silicon Native Execution

node_version=$(node -v)
echo "Current Node: $node_version"

CLAUDE_BIN=$(which claude)
ARCH_INFO=$(file "$CLAUDE_BIN")

if [[ "$ARCH_INFO" == *"arm64"* ]]; then
    echo "✅ Success: Running natively on Apple Silicon."
else
    echo "❌ Warning: Running via Rosetta (x86_64)."
fi
```

### Senior CLAUDE.md Template
```markdown
# CLAUDE.md

## 1. Architecture Context
- **Pattern**: DDD + Hexagonal
- **Stack**: TypeScript, React, PostgreSQL

## 2. Invariants (Constraints)
- Domain layer MUST NOT depend on Infrastructure.
- All async functions must return `Result<T, E>`.

## 3. Agent Protocol
- **Thinking**: Use `/think hard` for domain logic changes.
- **Read First**: Always read `src/domain` before implementing.
```

## 💡 Key Insights
- **React-in-Terminal**: 터미널 UI도 상태(State) 기반으로 동작한다는 점을 이해하면, 복잡한 사용자 상호작용 디자인이 가능하다.
- **Context Curation**: `CLAUDE.md`는 AI를 위한 README다. 사람이 읽는 문서와 달리, "원칙"과 "제약조건"을 명시하는 것이 훨씬 효과적이다.
- **Thinking Budget**: 모든 작업에 Deep Thinking이 필요하지 않다. 작업의 복잡도에 따라 토큰 예산을 할당하는 것이 비용 효율적이다.
