---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/frontend
  - tech_stack/react
  - library
  - state_management
Source: User Prompt
---

# Zustand

## Definition
**Zustand**는 React 애플리케이션을 위한 **빠르고 확장 가능한 상태 관리 라이브러리**입니다. 독일어로 "상태(State)"를 의미하며, 최소한의 API로 Flux 패턴을 구현합니다. 불필요한 보일러플레이트를 제거하고, **Hook 기반**의 직관적인 사용성을 제공하여 2026년 현재 가장 선호되는 솔루션 중 하나입니다.

## Key Features (핵심 특징)
- **Zero Boilerplate**: Provider가 필요 없으며, `create` 함수 하나로 스토어를 정의합니다.
- **Hook Based**: `useStore` 훅을 통해 컴포넌트 내에서 간편하게 상태를 구독합니다.
- **Performance**: Selector 패턴을 기본 지원하여 불필요한 리렌더링을 방지합니다.
- **Middleware Ecosystem**: `persist`(로컬스토리지), `devtools`(디버깅), `immer`(불변성) 등 강력한 미들웨어를 제공합니다.
- **Micro-Bundle**: 번들 크기가 약 **1KB** 내외로 매우 가볍습니다.

## Basic Usage
```tsx
import { create } from 'zustand';

interface CounterStore {
  count: number;
  increment: () => void;
  decrement: () => void;
  reset: () => void;
}

export const useCounterStore = create<CounterStore>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));

// 컴포넌트 사용
function Counter() {
    // Selector를 통한 최적화 (권장)
    const count = useCounterStore((state) => state.count);
    const increment = useCounterStore((state) => state.increment);
    return <button onClick={increment}>{count}</button>;
}
```

## Advanced Patterns
### 1. Persist Middleware (영속성)
브라우저 스토리지(localStorage 등)에 상태를 자동 저장/복구합니다.
```tsx
import { persist } from 'zustand/middleware';

export const useStore = create(
  persist(
    (set) => ({
      bears: 0,
      increase: () => set((state) => ({ bears: state.bears + 1 })),
    }),
    { name: 'bear-storage' } // 스토리지 Key
  )
);
```

### 2. Immer Middleware (불변성 관리)
Mutable한 문법으로 불변성을 관리할 수 있게 해줍니다.
```tsx
import { immer } from 'zustand/middleware/immer';

export const useStore = create(
  immer((set) => ({
    todos: [],
    addTodo: (text) =>
      set((state) => {
        state.todos.push({ text, done: false }); // Array.push 사용 가능
      }),
  }))
);
```

## Comparison (2026 Trend)
| Feature            | Zustand         | Redux Toolkit | Recoil / Jotai | Context API           |
| ------------------ | --------------- | ------------- | -------------- | --------------------- |
| **Boilerplate**    | Low             | Medium        | Low            | Medium                |
| **Learning Curve** | Low             | Medium        | Medium         | Low                   |
| **Performance**    | High (Selector) | High          | High (Atom)    | Low (Re-render issue) |
| **Usage**          | General         | Large Scale   | Atomic State   | Simple / Static       |

## Links
- [[React_MOC]]
