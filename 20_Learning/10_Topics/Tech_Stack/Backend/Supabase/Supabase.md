---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/backend
  - tech_stack/database
  - postgres
  - baas
Source: User Prompt
---

# Supabase

## Definition
**Supabase**는 **오픈소스 Firebase 대안**을 표방하는 **PostgreSQL 기반의 풀스택 백엔드 플랫폼(BaaS)**입니다. 2026년 기준, 데이터베이스(Postgres), 인증(Auth), 스토리지, 실시간(Realtime), 엣지 함수(Edge Functions), 그리고 **AI 벡터 검색(pgvector)**까지 통합된 가장 강력한 백엔드 솔루션으로 자리 잡았습니다.

## Key Features (2026 Trends)
- **True Postgres**: NoSQL이 아닌 표준 **PostgreSQL**을 제공하므로 SQL의 강력함과 확장성을 그대로 누릴 수 있습니다. (Row Level Security로 보안 통제)
- **Built-in AI**: **pgvector**가 기본 내장되어 있어 RAG(검색 증강 생성)나 임베딩 검색 같은 AI 기능을 별도 인프라 없이 구현합니다.
- **Realtime**: 데이터베이스 변경 사항을 WebSocket을 통해 실시간으로 클라이언트에 푸시합니다.
- **Auth & RLS**: JWT 기반의 강력한 인증 시스템이 DB의 Row Level Security와 직접 연동되어, 프론트엔드에서 안전하게 DB를 직접 조회할 수 있습니다.
- **Self-Hostable**: 모든 스택이 오픈소스이며, Docker Compose를 통해 온프레미스나 자체 클라우드에 쉽게 호스팅 가능합니다.

## Pricing (Pro Plan)
- 월 $25부터 시작 (프로젝트당). 
- 소규모 프로젝트는 Free Tier(500MB DB)로 충분, 스케일링 시 Pro로 전환 권장.

## Usage Example (Next.js Server Action)
```ts
// app/actions.ts
'use server';
import { createClient } from '@/utils/supabase/server';

export async function addTodo(formData: FormData) {
  const supabase = createClient();
  const { data, error } = await supabase
    .from('todos')
    .insert({ title: formData.get('title') })
    .select();
    
  if (error) throw error;
  return data;
}
```

## Comparison (vs Firebase)
| Feature     | Supabase            | Firebase            |
| ----------- | ------------------- | ------------------- |
| **DB**      | PostgreSQL (SQL)    | Firestore (NoSQL)   |
| **Query**   | Complex Joins       | Basic / Limited     |
| **Lock-in** | Zero (Standard SQL) | High                |
| **Trend**   | Rising (Enterprise) | Stable (Mobile/MVP) |

## Links
- [[Tech_Stack_MOC]]
- [[Nextjs]] (Best combination)
- [[PostgreSQL]]
