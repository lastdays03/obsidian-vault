---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/typescript
  - language
  - javascript
Source: User Prompt
---

# TypeScript

## Definition
**TypeScript**는 Microsoft가 개발한 JavaScript의 상위 집합(Superset)으로, **정적 타입 시스템**을 추가하여 코드의 안전성과 유지보수성을 극대화한 오픈소스 프로그래밍 언어입니다. 2026년 기준, 프론트엔드와 백엔드를 막론하고 현대 웹 개발의 **사실상 표준(De Facto Standard)**으로 자리 잡았습니다.

## Key Features (핵심 특징)
- **Static Typing**: 컴파일 시점에 타입을 검사하여 런타임 오류를 15~50% 감소시킵니다.
- **Developer Experience (DX)**: IDE에서의 자동 완성(IntelliSense), 실시간 에러 감지, 리팩토링 기능이 강력합니다.
- **Compatibility**: JavaScript와 100% 호환되며, 기존 프로젝트에 파일 단위로 점진적 도입(Gradual Adoption)이 가능합니다.
- **Ecosystem**: DefinitelyTyped(@types)를 통해 거의 모든 JavaScript 라이브러리의 타입을 지원합니다.
- **Scalability**: 대규모 프로젝트에서 코드베이스의 이해와 유지보수를 돕는 필수 도구입니다.

## Basic Usage
### 1. Type Annotation
```ts
// 기본 타입
const name: string = "TypeScript";
const age: number = 5;

// 객체 타입 (Interface)
interface User {
  id: number;
  username: string;
  role: 'admin' | 'user'; // Union Type
}

const user: User = {
  id: 1,
  username: "user1",
  role: "admin"
};
```

### 2. Generics (재사용성)
```ts
function identity<T>(arg: T): T {
  return arg;
}
const output = identity<string>("myString");
```

## Advanced Patterns (2026 Trends)
### 1. Template Literal Types
문자열 패턴을 강력하게 강제합니다.
```ts
type EventName = `on${string}`; // on으로 시작하는 모든 문자열
const onClick: EventName = "onClick";
```

### 2. Utility Types
기존 타입을 변형하여 재사용합니다.
- `Partial<T>`: 모든 속성을 선택적(Optional)으로 변경.
- `Pick<T, K>`: 특정 속성만 선택.
- `Omit<T, K>`: 특정 속성 제외.
```ts
type UserPreview = Pick<User, 'id' | 'username'>;
```

## Setup & Adoption
가장 좋은 도입 방법은 작은 프로젝트부터 **점진적으로** 적용하는 것입니다.
```bash
# React + Vite 프로젝트 시작 시
npm create vite@latest my-app -- --template react-ts
```

## Links
- [[Tech_Stack_MOC]]
- [[Frontend_MOC]] (Front-end usage)
