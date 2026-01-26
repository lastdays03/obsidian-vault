---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/testing
  - tool/vitest
  - frontend
Source: User Prompt
---

# Vitest

## Definition
**Vitest**는 Vite 기반의 차세대 JavaScript 단위 테스트 프레임워크입니다. Jest와 호환되는 API를 제공하면서도, Vite의 개발 서버 파이프라인을 공유하여 **극도로 빠른 속도(HMR)**와 **DX(Developer Experience)**를 제공합니다. 2026년 기준, Next.js 및 React 프로젝트의 표준 테스트 도구로 자리 잡았습니다.

## Key Features vs Jest (2026)
| Feature     | Vitest                 | Jest                          |
| ----------- | ---------------------- | ----------------------------- |
| **Speed**   | 🚀 Very Fast (Vite HMR) | 🐢 Slower (Cold start heavy)   |
| **Engine**  | Native ESM             | CommonJS (Translation needed) |
| **Config**  | Share `vite.config.ts` | Separate `jest.config.js`     |
| **Browser** | Native Browser Mode    | jsdom only (mostly)           |

## Setup (Next.js/React)
```bash
npm install -D vitest @vitejs/plugin-react jsdom @testing-library/react
```

### `vitest.config.ts`
```ts
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: './tests/setup.ts',
  },
});
```

## Basic Usage
```tsx
import { render, screen } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import Button from './Button';

describe('Button', () => {
  it('renders correctly', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button')).toHaveTextContent('Click me');
  });
});
```

## Links
- [[Testing_MOC]]
- [[Nextjs]] (Best Companion)
