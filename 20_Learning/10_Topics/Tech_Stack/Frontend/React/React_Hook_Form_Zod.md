---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/frontend
  - tech_stack/react
  - library
  - forms
  - validation
Source: User Prompt
---

# React Hook Form + Zod

## Definition
**React Hook Form**과 **Zod**의 조합은 2026년 기준 React/Next.js 프로젝트에서 폼 처리를 위한 **표준 스택**입니다. 비제어 컴포넌트(Uncontrolled Component) 기반인 React Hook Form의 **고성능**과, 스키마 선언형 유효성 검사 라이브러리인 Zod의 **강력한 타입 안전성**을 결합하여 최상의 개발자 경험을 제공합니다.

## Key Advantages
- **Performance**: 입력값 변경 시 리렌더링을 최소화합니다. (Uncontrolled Input 기본)
- **Type Safety**: Zod 스키마로부터 TypeScript 타입을 자동 추론(`z.infer`)하므로 별도의 인터페이스 정의가 불필요합니다.
- **Developer Experience**: `hooks` 기반의 간결한 API와 직관적인 에러 처리를 제공합니다.
- **Ecosystem**: Next.js Server Actions, TanStack Query와 매끄럽게 연동됩니다.

## Installation
```bash
npm install react-hook-form zod @hookform/resolvers
```

## Setup Pattern (Next.js + TS)
### 1. Schema Definition
```ts
// lib/schemas.ts
import { z } from 'zod';

export const loginSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
});

export type LoginForm = z.infer<typeof loginSchema>;
```

### 2. Form Component
```tsx
'use client';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { loginSchema, type LoginForm } from '@/lib/schemas';

export default function LoginForm() {
  const { register, handleSubmit, formState: { errors } } = useForm<LoginForm>({
    resolver: zodResolver(loginSchema),
  });

  const onSubmit = (data: LoginForm) => {
    console.log(data); // Fully typed data
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
        <input {...register("email")} placeholder="Email" />
        {errors.email && <p>{errors.email.message}</p>}
        
        <input type="password" {...register("password")} />
        <button type="submit">Login</button>
    </form>
  );
}
```

## Advanced Patterns
- **Server Actions**: `handleSubmit` 대신 `action` prop에 Server Action을 직접 연결할 수 있습니다. (Next.js 15+)
- **Dynamic Fields**: `useFieldArray`를 사용하여 동적으로 변하는 폼 필드를 쉽게 관리합니다.
- **Validation Reuse**: 정의한 Zod 스키마를 백엔드 API에서도 그대로 사용하여 유효성을 검증합니다. (DRY 원칙)

## Comparison
| Feature         | React Hook Form           | Formik         | React Final Form   |
| --------------- | ------------------------- | -------------- | ------------------ |
| **Re-renders**  | Minimal (Isolated)        | High (Context) | Low (Subscription) |
| **Size**        | Tiny (~10KB)              | Medium         | Medium             |
| **Zod Support** | First-class (`resolvers`) | Manual         | Manual             |

## Links
- [[React_MOC]]
- [[Nextjs]]
