---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/javascript
  - language
  - web
Source: User Prompt
---

# JavaScript

## Definition
**JavaScript**는 웹 브라우저의 핵심 스크립트 언어로 시작하여, 현재는 서버(Node.js), 모바일, 데스크톱까지 아우르는 범용 프로그래밍 언어입니다. 2026년 기준 **ECMAScript 2025(ES2025)** 표준이 널리 채택되었으며, TypeScript와 결합하여 대규모 애플리케이션 개발의 표준으로 자리 잡았습니다.

## Key Features (2026 Trends)
- **Universal Language**: 프론트엔드(React, Next.js)와 백엔드(Node.js, Deno, Bun)를 모두 아우르는 거대한 생태계를 가집니다.
- **Asynchronous Mastery**: `Promise`, `async/await`, `top-level await`를 통해 비동기 처리를 직관적으로 수행합니다.
- **ES2025 Standard**: `Iterator Helpers`, 새로운 `Set` 메서드(합집합, 교집합 등), `JSON modules` 등 개발 편의성이 대폭 강화되었습니다.
- **Dynamic Typing**: 런타임 유연성을 제공하나, 실무에서는 안전성을 위해 **TypeScript**와 함께 사용하는 것이 기본입니다.

## Important Concepts
1. **Event Loop**: 싱글 스레드 환경에서 비동기 작업을 처리하는 핵심 메커니즘 (Microtask vs Macrotask 이해 필수).
2. **Prototype Chain**: 클래스 문법 내부에서 동작하는 객체 상속 모델.
3. **Module System (ESM)**: `import`/`export`를 사용하는 표준 모듈 시스템 (Node.js에서도 기본 권장).

## Examples (ES2025+)
### 1. Iterator Helpers
```js
const numbers = [1, 2, 3, 4, 5].values();
const result = numbers.map(x => x * 2).filter(x => x > 5).take(2);
console.log([...result]); // [6, 8]
```

### 2. Set Operations
```js
const setA = new Set([1, 2, 3]);
const setB = new Set([3, 4, 5]);
const intersection = setA.intersection(setB); // Set {3}
```

## Comparisons
| Feature      | JavaScript        | TypeScript      | Python              |
| ------------ | ----------------- | --------------- | ------------------- |
| **Typing**   | Dynamic           | Static          | Dynamic (Type Hint) |
| **Runtime**  | Browser / Node.js | Transpile to JS | Python VMs          |
| **Use Case** | Web Standard      | Large Scale App | AI / Data / Backend |

## Links
- [[Tech_Stack_MOC]]
- [[TypeScript]] (Superset)
