---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/frontend
  - tech_stack/react
  - framework
  - ssr
Source: User Prompt
---

# Next.js

## Definition
**Next.js**는 Vercel이 개발한 **React 기반의 풀스택 웹 프레임워크**입니다. "The React Framework for the Web"을 표방하며, React의 모든 최신 기능(RSC, Suspense)을 프로덕션 레벨에서 사용할 수 있게 해줍니다. 2026년 기준, **App Router**와 **Server Components**를 중심으로 웹 개발의 표준으로 자리 잡았습니다.

## Key Features (핵심 특징 - 2026년 기준)
- **App Router**: 파일 시스템 기반 라우팅(Layouts, Nested Routes, Loading UI)을 지원하며, React Server Components(RSC)가 기본입니다.
- **Rendering Modes**:
    - **SSR (Server-Side Rendering)**: 요청 시 서버에서 렌더링.
    - **SSG (Static Site Generation)**: 빌드 시 정적 생성.
    - **ISR (Incremental Static Regeneration)**: 정적 페이지를 주기적으로 갱신.
    - **PPR (Partial Prerendering)**: 정적 껍데기(Shell)와 동적 구멍(Hole)을 결합하여 최적의 성능 제공.
- **Server Actions**: API 라우트 없이 서버 함수를 직접 호출하여 폼 처리 및 데이터 변형(Mutation)을 수행합니다.
- **Turbopack**: Rust 기반의 초고속 번들러로, Webpack 대비 압도적인 개발 속도를 자랑합니다.
- **Optimization**: 이미지(`next/image`), 폰트(`next/font`), 스크립트 최적화를 자동 수행합니다.

## Installation
```bash
npx create-next-app@latest my-app
# Options: TypeScript (Yes), Tailwind (Yes), App Router (Yes), src/ (Yes)
```

## Basic Structure (App Router)
`Page`는 기본적으로 **Server Component**입니다.

```tsx
// app/page.tsx
import Link from 'next/link';

export default function Home() {
  return (
    <main className="p-24">
      <h1 className="text-4xl font-bold">Welcome to Next.js 16</h1>
      <Link href="/dashboard" className="text-blue-500 hover:underline">
        Go to Dashboard
      </Link>
    </main>
  );
}
```

## Client Component
상호작용(Hooks, Event Listeners)이 필요한 경우 `'use client'` 지시어를 사용합니다.

```tsx
// app/counter/page.tsx
'use client';

import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(c => c + 1)}>
      Count: {count}
    </button>
  );
}
```

## Use Cases (언제 사용하는가?)
- **SEO가 중요한 서비스**: 검색 엔진 최적화가 필수인 커머스, 블로그, 랜딩 페이지.
- **풀스택 애플리케이션**: 프론트엔드와 백엔드(API Routes, Server Actions)를 하나의 프로젝트로 관리할 때.
- **고성능 웹사이트**: 초기 로딩 속도(LCP)와 사용자 경험(CLS)이 중요한 경우.

## Links
- [[React_MOC]]
- [[Frontend_MOC]]
