---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/testing
  - dev-process
  - quality-assurance
Source: User Prompt
---

# Software Testing

## Definition
**소프트웨어 테스트**는 애플리케이션이 요구사항을 충족하고 버그 없이 동작함을 보장하는 활동입니다. 2026년 기준, **Next.js + React 풀스택** 환경에서는 **Vitest(단위)**, **Playwright(E2E)**, **MSW(API Mocking)** 조합이 표준으로 자리 잡았으며, AI 기반 테스트 자동화가 활발히 도입되고 있습니다.

## Testing Pyramid (2026 Standard)
1. **Unit Test (60-70%)**: Vitest, React Testing Library. 개별 함수 및 컴포넌트 동작 검증. 빠르고 저렴함.
2. **Integration Test (20-30%)**: Vitest + MSW. 여러 모듈/컴포넌트 간 상호작용 및 데이터 흐름 검증.
3. **E2E Test (5-15%)**: Playwright. 실제 사용자 흐름(로그인~결제)을 브라우저 상에서 검증. 느리고 비쌈.

## Key Tools (Frontend Focus)
- **[[Vitest]]**: Vite 기반의 초고속 유닛 테스트 프레임워크 (Jest 대체).
- **Playwright**: 모든 최신 브라우저를 지원하는 빠르고 강력한 E2E 도구 (Cypress 대체).
- **React Testing Library (RTL)**: 구현 상세가 아닌 사용자 관점("버튼을 클릭한다")에서의 테스트 작성을 강제함.
- **MSW (Mock Service Worker)**: 네트워크 레벨에서 API 요청을 가로채서 모의 응답(Mock)을 제공.

## Testing Strategy Example
```tsx
// 1. Unit (Vitest + RTL)
render(<Button>Click</Button>);
expect(screen.getByRole('button')).toBeInTheDocument();

// 2. Integration (MSW)
server.use(http.get('/api/user', () => HttpResponse.json({ name: 'User' })));
render(<UserProfile />);
await screen.findByText('User');

// 3. E2E (Playwright)
await page.goto('/login');
await page.click('button[type="submit"]');
await expect(page).toHaveURL('/dashboard');
```

## Links
- [[Tech_Stack_MOC]]
- [[Nextjs]] (Testing Integrated)
