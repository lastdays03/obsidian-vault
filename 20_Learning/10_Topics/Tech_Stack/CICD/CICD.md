---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/cicd
  - devops
  - dev-process
Source: User Prompt
---

# CI/CD (Continuous Integration / Continuous Deployment)

## Definition
**CI/CD**는 코드 변경을 자동으로 빌드·테스트하고 배포하는 파이프라인으로, 현대 개발 프로세스의 필수 요소입니다. 2026년 기준, **GitHub Actions**가 표준으로 자리 잡았으며, **Vercel/Cloudflare Pages**와 결합된 "Push-to-Deploy" 방식과 **GitOps**가 주류입니다.

## CI/CD Pipeline Stages (2026 Standard)
1. **CI (Continuous Integration)**: 코드 통합 및 검증 (Lint → Unit Test → Build).
2. **CD (Continuous Delivery)**: 배포 가능한 아티팩트(Docker 이미지 등) 생성 및 스테이징 자동 배포.
3. **CD (Continuous Deployment)**: 모든 검증을 통과한 코드를 프로덕션까지 자동 배포 (Continuous Deployment).

## Popular Combinations
| Stack             | CI Tool        | CD Platform         | Ideal For                   |
| ----------------- | -------------- | ------------------- | --------------------------- |
| **Next.js**       | GitHub Actions | Vercel              | Frontend / Fullstack        |
| **Edge App**      | GitHub Actions | Cloudflare Pages    | High Performance / Low Cost |
| **Microservices** | GitHub Actions | ArgoCD + Kubernetes | Enterprise Scale            |

## GitHub Actions Example (Next.js)
```yaml
name: CI
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20 }
      - run: npm ci
      - run: npm run test
```

## Links
- [[Tech_Stack_MOC]]
- [[Testing]] (Pre-requisite for CI)
- [[Docker]] (Artifact for CD)
