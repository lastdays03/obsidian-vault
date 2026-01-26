---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/infrastructure
  - devops
  - tool/docker-compose
Source: User Prompt
---

# Docker Compose

## Definition
**Docker Compose**는 단일 YAML 파일(`compose.yaml`)로 여러 개의 컨테이너(Multi-container)를 정의하고 실행하는 Docker 공식 도구입니다. 복잡한 애플리케이션 스택(예: Next.js + DB + Redis)을 코드(Infrastrucure as Code)로 정의하여, 개발자가 **"한 번의 명령(`docker compose up`)"**으로 전체 환경을 동일하게 실행할 수 있게 합니다.

## Key Features (2026 Trends)
- **Single YAML**: 모든 서비스 설정을 파일 하나로 관리하여 재현성을 보장합니다.
- **Development Productivity**: `watch` 모드(v2.22+)를 통해 소스 코드 변경 시 컨테이너를 자동으로 재빌드하거나 동기화하여 개발 속도를 높입니다.
- **Profiles**: `--profile dev` 등을 통해 개발/테스트/운영 환경에 따라 실행할 서비스 그룹을 분리할 수 있습니다.
- **Built-in**: Docker Desktop 및 Engine 최신 버전에 플러그인(`docker compose`) 형태로 기본 내장되어 있습니다.

## Basic Usage
```yaml
# compose.yaml
services:
  web:
    build: .
    ports: ["3000:3000"]
    develop:
      watch:
        - action: sync
          path: ./app
          target: /app/app
  db:
    image: postgres:16
    volumes: ["db-data:/var/lib/postgresql/data"]

volumes:
  db-data:
```

## Essential Commands
- `docker compose up -d`: 백그라운드에서 실행.
- `docker compose down`: 컨테이너 및 네트워크 정리.
- `docker compose logs -f`: 로그 실시간 확인.
- `docker compose watch`: 파일 변경 감지 및 자동 동기화 (Hot Reload).

## Links
- [[Docker_MOC]]
- [[Docker]] (Parent Tool)
