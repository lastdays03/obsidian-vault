---
tags: [concept/devops, tool/docker]
Up: [[CS_Concepts_MOC]]
---

# DevContainers

> **DevContainers (Development Containers)**는 프로젝트의 개발 환경(컴파일러, 런타임, 도구, 확장 프로그램 등)을 **도커 컨테이너** 내부에 정의하여 코드로 관리하는 기술입니다.

## 📖 Definition
"내 컴퓨터에서는 되는데?"라는 고질적인 문제를 해결하기 위한 기술입니다. 
VS Code의 `devcontainer.json`을 사용하여, 프로젝트를 열 때마다 사전에 정의된 도커 이미지를 기반으로 일관된 개발 환경을 구축합니다.
최근에는 Claude Code와 같은 **AI 에이전트의 실행 환경을 격리(Sandboxing)**하는 보안 용도로도 중요하게 사용됩니다.

## 💻 Example
Claude Code 샌드박싱을 위한 설정 예시:

```json
{
  "name": "Node.js Sandbox",
  "image": "mcr.microsoft.com/devcontainers/typescript-node:20",
  "features": {
    "ghcr.io/devcontainers/features/docker-in-docker:2": {}
  },
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": ["anthropic.claude-code"]
    }
  },
  "remoteEnv": {
    "ANTHROPIC_API_KEY": "${localEnv:ANTHROPIC_API_KEY}"
  }
}
```

## 🆚 Comparison
| 구분 | Virtual Environment (venv) | Virtual Machine (VM) | DevContainer |
| :--- | :--- | :--- | :--- |
| **격리 수준** | 라이브러리 격리 | OS 전체 격리 | 프로세스/파일시스템 격리 |
| **자원 소모** | 낮음 | 매우 높음 | 중간 (Docker) |
| **이식성** | 낮음 (OS 의존적) | 높음 | 매우 높음 |

## 🔗 Connected Concepts
- [[Docker]]
- [[Infrastructure as Code (IaC)]]
- [[Sandboxing]]

Source: [[05_Enterprise_Architecture]]
