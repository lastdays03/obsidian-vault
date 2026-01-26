---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/infrastructure
  - tech_stack/container
  - devops
  - tool/docker
Source: User Prompt
---

# Docker

## Definition
**Docker**는 애플리케이션과 실행 환경을 **컨테이너**라는 표준화된 단위로 패키징하여, 개발, 테스트, 운영 환경 간의 차이를 없애고 일관된 실행을 보장하는 플랫폼입니다. 2026년 기준, 클라우드 네이티브 생태계의 기반이자 AI 모델 배포의 표준으로 여전히 확고한 위치를 차지하고 있습니다.

## Key Concepts
- **Container**: 격리된 실행 환경 (프로세스 + 파일시스템). VM보다 훨씬 가볍고 빠릅니다.
- **Image**: 컨테이너 실행을 위한 읽기 전용 템플릿. (Dockerfile로 빌드)
- **Dockerfile**: 인프라를 코드로 정의(IaC)한 레시피.
- **Docker Compose**: 멀티 컨테이너 애플리케이션(App + DB 등)을 YAML로 정의하고 실행합니다.
- **Volume**: 컨테이너가 삭제되어도 데이터를 유지하기 위한 영속성 저장소입니다.

## 2026 Trends
- **AI/ML Integration**: **Docker Models** 등을 통해 대규모 LLM이나 ML 모델을 표준화된 방식으로 배포하고 실행하는 기능이 강화되었습니다.
- **Hardened Security**: 프로덕션 레벨의 보안이 적용된 경량화 이미지(Distroless, Alpine 등) 사용이 표준화되었습니다.
- **Dev Environments**: 로컬 개발 환경을 컨테이너화하여 팀원 간 "내 컴퓨터에서는 되는데..." 문제를 원천 차단합니다.

## Basic Usage
```bash
# Build
docker build -t my-app .

# Run
docker run -d -p 3000:3000 my-app

# Compose (Multi-container)
docker compose up -d
```

## Comparison
| Feature  | Docker             | Podman               | Kubernetes       |
| -------- | ------------------ | -------------------- | ---------------- |
| **Role** | Runtime & Build    | Runtime (Daemonless) | Orchestration    |
| **User** | Developer Standard | Security Focus       | Ops / Production |

## Links
- [[Tech_Stack_MOC]]
- [[73_ML_Course_Docker]] (ML Context)
- [[00_MCP_Docker_Deployment_Strategy]] (Deployment Context)
