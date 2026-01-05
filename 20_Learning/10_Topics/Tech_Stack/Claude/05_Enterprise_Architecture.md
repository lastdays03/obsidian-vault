---
tags: [knowledge/topic, tool/claude, architecture/enterprise]
Up: [[Claude_Code_MOC]]
---

# 05. Enterprise Architecture

엔터프라이즈 환경에서 AI 코딩 도구를 도입할 때의 핵심은 **보안(Security)**과 **코드 품질(Quality Assurance)**입니다. 이를 위해 **[[DevContainers|Docker Sandboxing]]**과 **[[Agentic_TDD|Agentic TDD]]**를 결합한 아키텍처를 제시합니다.

## 🛡️ DevContainers 샌드박싱

Claude Code가 호스트 OS의 민감 정보에 접근하는 것을 원천 차단하기 위해, 실행 환경을 도커 컨테이너 내부로 격리합니다.

### Security Configurations
- **Isolation**: 파일 시스템 접근을 컨테이너 내부(`/workspace`)로 제한.
- **Credential Injection**: API Key 등은 `remoteEnv`를 통해서만 안전하게 주입.
- **Network**: 필요 시 Docker Network 설정을 통해 아웃바운드 트래픽 제어 가능.

### `devcontainer.json` Snippet
```json
{
  "name": "Claude Code Sandbox",
  "image": "mcr.microsoft.com/devcontainers/typescript-node:20",
  "features": {
    "ghcr.io/devcontainers/features/docker-in-docker:2": {}
  },
  "postCreateCommand": "npm install -g @anthropic-ai/claude-code",
  "remoteEnv": {
    "ANTHROPIC_API_KEY": "${localEnv:ANTHROPIC_API_KEY}"
  }
}
```

## 💻 Agentic TDD Protocol

AI가 스스로 테스트를 작성하고 통과시키는 **Red-Green-Refactor** 루프입니다.

### Agentic TDD Skill
```yaml
---
name: agentic-tdd
description: Implements features using a strict Red-Green-Refactor cycle.
allowed-tools: ["bash", "write_file", "sequential-thinking"]
---

# Agentic TDD Protocol

## Philosophy
We do NOT write implementation code until a failing test confirms the requirement.

## Phase 1: RED (Write Test)
1. Write a test file describing expected behavior.
2. Run test -> it MUST FAIL.

## Phase 2: GREEN (Make it Pass)
1. Write minimum code to satisfy the test.
2. Run test -> it MUST PASS.

## Phase 3: REFACTOR
1. Clean up code without changing behavior.
2. Verify pass.
```

## 💡 Key Insights
- **Containment Strategy**: AI는 강력하지만 위험할 수 있다. 샌드박스는 AI가 마음껏 뛰어놀 수 있는 "안전한 놀이터"를 제공한다.
- **Code Reliability**: "작동하는 코드"와 "올바른 코드"는 다르다. TDD를 통해 AI는 "검증된 코드"만을 생산하게 된다.
- **Autonomous QA**: 인간이 테스트를 작성하는 것이 아니라, AI가 테스트를 작성하고 스스로를 검증하게 하는 것이 진정한 에이전트 워크플로우다.
