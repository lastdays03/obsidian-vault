---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/backend
  - tech_stack/database
  - nosql
  - baas
Source: User Prompt
---

# Firebase

## Definition
**Firebase**는 Google이 제공하는 **서버리스 백엔드 플랫폼(BaaS)**으로, 모바일(iOS/Android) 및 웹 애플리케이션의 개발, 배포, 확장을 위한 올인원 솔루션입니다. 2011년 시작 이후 2026년 현재 **AI 통합(Gemini/Vertex AI)**과 **서버리스 중심**의 확장이 강화되었으며, 특히 실시간 동기화와 오프라인 지원이 필요한 앱 개발에 강력합니다.

## Key Features (2026 Trends)
- **Firestore**: 문서-컬렉션 구조의 NoSQL 데이터베이스로, 실시간 동기화와 오프라인 캐싱이 업계 최고 수준입니다.
- **Authentication**: Google, Apple, 익명 등 10개 이상의 인증 제공자를 무료(SMS 제외)로 지원합니다.
- **Cloud Functions (Gen 2)**: Python, Go 등을 지원하며 성능이 대폭 개선된 서버리스 함수 환경을 제공합니다.
- **AI/ML Integration**: Gemini 및 Vertex AI와 네이티브하게 연동되어 생성형 AI 기능을 쉽게 추가할 수 있습니다.
- **Offline First**: 모바일 환경에서 네트워크 끊김 시에도 앱이 정상 동작하도록 돕는 강력한 SDK를 내장하고 있습니다.

## Pricing (Spark vs Blaze)
- **Spark (Free)**: 학습 및 소규모 프로젝트에 충분한 넉넉한 무료 티어 제공 (Auth 무제한 등).
- **Blaze (Pay-as-you-go)**: 사용량 기반 과금. 복잡한 앱의 경우 Firestore 읽기/쓰기 비용이 빠르게 증가할 수 있어 주의 필요.

## Comparison (vs Supabase)
| Feature      | Firebase          | Supabase         |
| ------------ | ----------------- | ---------------- |
| **DB**       | NoSQL (Firestore) | PostgreSQL (SQL) |
| **Realtime** | Best in Class     | Great (Changes)  |
| **Offline**  | Superior (Native) | Improving        |
| **Standard** | Proprietary       | Open Source      |

## Usage Example (Next.js)
```tsx
import { initializeApp } from 'firebase/app';
import { getFirestore, collection, addDoc } from 'firebase/firestore';

const app = initializeApp({ /* config */ });
const db = getFirestore(app);

// Server Action
export async function createPost(data) {
  'use server';
  await addDoc(collection(db, 'posts'), data);
}
```

## Links
- [[Tech_Stack_MOC]]
- [[Supabase]] (Major Competitor)
