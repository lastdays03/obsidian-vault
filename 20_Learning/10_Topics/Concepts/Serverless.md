---
created: 2026-01-26
tags:
  - knowledge/concept
  - cloud_computing
  - serverless
  - architecture
Up: [[Concepts_MOC]]
Source: User Prompt
---

# Serverless (서버리스)

## Definition
**Serverless**는 개발자가 서버 인프라를 직접 관리하거나 프로비저닝하지 않고도 애플리케이션과 서비스를 구축하고 실행할 수 있게 해주는 클라우드 컴퓨팅 패러다임입니다. "서버가 없다"는 뜻이 아니라, 서버 관리에 대한 책임을 클라우드 제공업체가 전담한다는 의미입니다.

## Key Core Concepts
- **[[FaaS]] (Function as a Service)**: 코드 조각(함수) 단위로 배포하고 이벤트가 발생할 때만 실행됩니다. (예: AWS Lambda)
- **BaaS (Backend as a Service)**: 데이터베이스, 인증, 스토리지 등 백엔드 기능을 API로 제공받아 사용합니다. (예: [[Supabase]], Firebase)
- **Event-Driven**: HTTP 요청, 데이터베이스 변경, 파일 업로드 등 특정 이벤트에 반응하여 작동합니다.
- **Pay-as-you-go**: 코드가 실행되는 시간과 자원 사용량에 대해서만 비용을 지불합니다. (Idle 상태 비용 0)

## Pros & Cons (2026)
| Pros                               | Cons                           |
| ---------------------------------- | ------------------------------ |
| 운영 부담 거의 없음 (Auto-scaling) | **Cold Start** 지연 발생 가능  |
| 효율적인 비용 (사용한 만큼만 점유) | 벤더 종속성 (Vendor Lock-in)   |
| 빠른 출시 속도 (Time-to-Market)    | 디버깅 및 로컬 테스트의 복잡성 |

## Major Services
- **Full Stack**: Vercel, Netlify
- **Functions**: AWS Lambda, Cloudflare Workers, [[Supabase]] Edge Functions
- **Database**: [[Serverless_Database|PlanetScale, Neon, Upstash]]

## Links
- [[Concepts_MOC]]
- [[Tech_Stack_MOC]]
- [[PaaS]] (Serverless lives here)
- [[Serverless_Database]]
