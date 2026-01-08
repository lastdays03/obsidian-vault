---
tags: [knowledge/topic, tool/claude]
Up: [[Claude_Code_MOC]]
---

# 02. MCP Ecosystem & Skills

**[[Model_Context_Protocol|MCP (Model Context Protocol)]]**는 Claude가 외부 세계와 상호작용하기 위한 **표준 인터페이스**이며, **Skill**은 특정 작업을 수행하는 **절차적 지식**입니다.

## 📖 MCP vs Skill

- **MCP (Hands & Feet)**: 
    - 역할: 외부 도구(Filesystem, DB, Browser)에 대한 접근 권한을 제공하는 드라이버.
    - 예시: "Postgres에 접속해서 쿼리를 날려줘."
- **Skill (Manual)**: 
    - 역할: `.claude/skills`에 정의된 작업 절차서. 아키텍처 패턴을 강제하거나 표준화된 워크플로우를 가이드함.
    - 예시: "이 코드를 작성하기 전에 반드시 아키텍처 검증을 수행해."

## 🛠️ 필수 MCP 서버 Top 5

| 서버 | 역할 | 활용 포인트 |
| :--- | :--- | :--- |
| **Filesystem** | 로컬 파일 접근 | 샌드박싱 필수 (Project Root 제한) |
| **GitHub** | 협업 워크플로우 | PR 생성, 코드 리뷰, 이슈 트래킹 |
| **Postgres** | DB 핸들링 | 스키마 조회, 읽기 전용 쿼리 (ERD 대체) |
| **Sequential Thinking** | 사고 과정 구조화 | 복잡한 문제의 단계적 분해 (Chain of Thought) |
| **Browser** | 웹 문서 학습 | 최신 라이브러리 공식 문서의 실시간 학습 |

## 💻 Custom Skill 제작

### Architecture Validator
개발자가 코드를 작성하기 전에 아키텍처 위반 여부를 스스로 검사하도록 강제하는 스킬입니다.

**File Path**: `.claude/skills/architecture-validator/SKILL.md`

```yaml
---
name: architecture-validator
description: Validates code changes against Hexagonal Architecture rules.
allowed-tools: ["read_file", "grep", "sequential-thinking"]
---

# Architecture Validator Skill

## Purpose
Ensure NO dependency rules are violated. Specifically prevents UI/Infrastructure logic from leaking into Domain layers.

## Validation Protocol
1. **Dependency Direction Check**:
   - Files in `src/domain` MUST NOT import from `src/infrastructure` or `src/ui`.
   - Command: `grep -r "from.*infrastructure" src/domain` -> Output MUST be empty.

2. **Interface Implementation Check**:
   - Infrastructure adapters MUST implement interfaces defined in `src/domain`.

3. **Thinking Process**:
   - Use `sequential-thinking` to evaluate separation of concerns.

## Enforcement
If violation found: STOP immediately and report.
```

## ⚙️ .mcp.json 설정

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "./src"]
    },
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

## 💡 Key Insights
- **MCP as Drivers**: MCP는 OS의 디바이스 드라이버와 같다. LLM이 세상과 소통하는 통로를 표준화한다.
- **Skill as Guardrails**: Skill은 단순한 프롬프트가 아니라, 주니어 개발자의 실수를 방지하는 강력한 가드레일(Guardrail)이다.
- **Sequential Thinking**: AI에게 "천천히 생각하라"고 강제하는 것은 디버깅과 설계 단계에서 비약적인 성능 향상을 가져온다.
