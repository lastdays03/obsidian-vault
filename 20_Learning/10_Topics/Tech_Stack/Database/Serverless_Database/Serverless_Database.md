---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/database
  - serverless
  - cloud_computing
Source: User Prompt
---

# Serverless Database

## Definition
**Serverless Database**는 개발자가 서버 관리(프로비저닝, 패치, 백업) 없이 사용량에 따라 자동으로 스케일링되며 과금되는 데이터베이스 서비스입니다. 2026년 기준, **Supabase, Neon, PlanetScale** 등이 AI 및 모던 웹 앱의 표준 데이터 저장소로 사용됩니다.

## Key Advantages
- **Zero Maintenance**: OS 패치, 백업, 복제 등을 공급자가 전담.
- **Auto-Scaling**: 트래픽에 따라 스토리지와 컴퓨팅 리소스가 자동 확장/축소 (Scale to Zero 지원).
- **Consumption-Based Pricing**: 실제 사용한 만큼만 지불하여 유휴 비용 절감.
- **Global Distribution**: 엣지 로케이션을 통한 저지연 글로벌 서비스 지원.

## Top Providers & Comparison (2026)
| Provider        | Type       | Best For             | Note                                    |
| --------------- | ---------- | -------------------- | --------------------------------------- |
| **Supabase**    | PostgreSQL | Fullstack / Realtime | Auth/Storage 통합, pgvector 기본        |
| **Neon**        | PostgreSQL | Branching / Dev Exp  | Serverless Postgres의 표준, 브랜칭 기능 |
| **PlanetScale** | MySQL      | Massive Scale        | Sharding 자동화 (Vitess), 브랜칭 지원   |
| **DynamoDB**    | NoSQL      | AWS Ecosystem        | 극강의 스케일, 키-값 저장소             |
| **Firestore**   | NoSQL      | Mobile / Offline     | 앱용 실시간 DB, 오프라인 지원           |

## Selection Guide
- **Relation + Fullstack**: [[Supabase]] or [[Neon]]
- **MySQL + Scale**: PlanetScale
- **Mobile/Offline**: Firestore
- **Simple NoSQL**: DynamoDB

## Links
- [[Database_MOC]]
- [[Supabase]] (Representative Tool)
- [[SaaS]] (Business Model)
