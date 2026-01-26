---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/testing
  - tool/jsdom
  - environment/simulation
Source: User Prompt
---

# jsdom

## Definition
**jsdom**은 Node.js 환경에서 사용하기 위해 순수 JavaScript로 구현된 웹 표준(WHATWG DOM, HTML 표준)의 하위 집합입니다. 2026년 현재, **React/Vue/Svelte 컴포넌트 단위 테스트** 시 **브라우저 없이도 DOM API(`document`, `window` 등)를 사용**할 수 있게 해주는 가장 표준적인 환경입니다.

## Key Features
- **DOM Simulation**: HTML 파싱 및 DOM 트리 생성, 이벤트 전파(Event Bubbling) 등을 시뮬레이션합니다.
- **Node.js Integration**: Vitest나 Jest 같은 Node.js 기반 테스트 러너에서 전역 `window`, `document` 객체를 주입합니다.
- **Speed**: 실제 브라우저(Chrome Headless 등)를 띄우는 것보다 훨씬 가볍고 빠릅니다. (단, 레이아웃 계산은 하지 않음)

## Setup (Vitest)
```ts
// vite.config.ts
export default defineConfig({
  test: {
    environment: 'jsdom', // or 'happy-dom'
  },
});
```

## Comparisons (2026)
| Env              | Speed   | Fidelity                   | Use Case                    |
| ---------------- | ------- | -------------------------- | --------------------------- |
| **jsdom**        | Fast    | High (Standard Compliance) | General React Unit Tests    |
| **happy-dom**    | Fastest | Medium                     | Speed-critical Utility Libs |
| **Browser Mode** | Slow    | 100%                       | Visual / E2E / Complex DOM  |

## Links
- [[Testing_MOC]]
- [[Vitest]] (Consumer)
