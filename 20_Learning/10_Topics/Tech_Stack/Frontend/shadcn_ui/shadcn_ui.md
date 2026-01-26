---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/frontend
  - tech_stack/ui
  - tech_stack/react
  - tailwind_css
Source: User Prompt
---

# shadcn/ui

## Definition
**shadcn/ui**는 2026년 기준 React/Next.js 생태계에서 **가장 인기 있는 UI 컴포넌트 시스템**입니다. 전통적인 npm 라이브러리 방식이 아닌, **"Copy-Paste" 방식**을 채택하여 소유권과 커스터마이징의 자유를 극대화했습니다. **Radix UI** (또는 Base UI)의 견고한 접근성과 **Tailwind CSS**의 유연한 스타일링을 결합하여 아름답고 접근성 높은 컴포넌트를 제공합니다.

## Key Features (핵심 특징)
- **Copy-Paste Architecture**: `npx shadcn@latest add` 명령어로 컴포넌트 소스 코드를 프로젝트(`./components/ui`)에 직접 설치합니다.
- **Full Control**: 설치된 컴포넌트 코드는 100% 내 코드이므로, 디자인 시스템에 맞춰 자유롭게 수정할 수 있습니다.
- **Headless Base**: Radix UI 또는 Base UI를 기반으로 하여 WAI-ARIA 접근성과 키보드 네비게이션을 완벽 지원합니다.
- **Tailwind Native**: 별도의 CSS 파일 없이 오직 Tailwind 유틸리티 클래스로만 스타일링합니다.
- **Theming**: CSS 변수를 활용하여 다크 모드와 테마 색상 변경을 손쉽게 처리합니다.

## Installation
```bash
npx shadcn@latest init
# 질문에 따라 Style(Default/New York), Base color, TS, Tailwind 등을 설정
```

## Adding Components
```bash
npx shadcn@latest add button card dialog
```

## Usage Example
```tsx
import { Button } from "@/components/ui/button"
import { Card, CardHeader, CardTitle, CardContent } from "@/components/ui/card"

export default function LoginCard() {
  return (
    <Card className="w-[350px]">
      <CardHeader>
        <CardTitle>Login</CardTitle>
      </CardHeader>
      <CardContent>
        <Button variant="outline" className="w-full">
            Sign in with Google
        </Button>
      </CardContent>
    </Card>
  )
}
```

## Comparison (2026 Trend)
| Feature           | shadcn/ui              | MUI (Material UI) | Chakra UI         |
| ----------------- | ---------------------- | ----------------- | ----------------- |
| **Installation**  | Copy Code              | npm package       | npm package       |
| **Styling**       | Tailwind CSS           | CSS-in-JS         | CSS-in-JS / Panda |
| **Customization** | Full (Source Edit)     | Theme Override    | Theme Extend      |
| **Bundle Size**   | Minimal (Tree-shaking) | Large             | Medium            |

## Links
- [[Frontend_MOC]]
- [[Tailwind_CSS]] (Base Styling)
- [[React_Hook_Form_Zod]] (Best Combination)
