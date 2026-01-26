---
created: 2026-01-26
tags:
  - knowledge/concept
  - cloud_computing
  - serverless
  - faas
Up: [[Concepts_MOC]]
Source: User Prompt
---

# FaaS (Function as a Service)

## Definition
**FaaS**는 서버리스 컴퓨팅의 핵심 구현 방식으로, 애플리케이션 로직을 독립된 함수 단위로 배포하고 이벤트가 발생할 때만 실행 및 스케일링하는 서비스입니다. 2026년 기준, 인프라 관리 부담을 최소화하려는 현대적 아키텍처의 표준으로 자리 잡았습니다.

## Key Platforms Comparison (2026)
| Platform               | Target           | Cold Start | Standout Feature        |
| ---------------------- | ---------------- | ---------- | ----------------------- |
| **AWS Lambda**         | Complex Backend  | 100~800ms  | Ecosystem & Reliability |
| **Cloudflare Workers** | Edge / Global    | ~0ms       | Fastest, V8 Isolates    |
| **Vercel Functions**   | Full-stack Web   | 0~100ms    | Next.js Optimization    |
| **Supabase Functions** | BaaS Integration | 50~300ms   | Deno-based, DB-native   |

## Selection Guide
- **Low Latency**: Cloudflare Workers (Edge Computing)
- **Complex / Deep Infra**: AWS Lambda
- **Next.js Developer**: Vercel Functions
- **Database Centric**: [[Supabase]] Edge Functions

## Cold Start Reality
- **Cold Start**: 함수가 처음 호출될 때 발생하는 지연 (런타임 로드 시간).
- **Warm Start**: 이미 로드된 인스턴스를 재사용하여 즉시 실행.
- **Optimization**: Provisioned Concurrency (AWS), SnapStart, or using Edge Runtimes (Cloudflare) to minimize delay.

## Links
- [[Serverless]] (Parent Concept)
- [[Concepts_MOC]]
- [[Supabase]]
- [[Vercel]]
