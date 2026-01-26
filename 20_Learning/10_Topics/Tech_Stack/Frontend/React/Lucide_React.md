---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/frontend
  - library
  - react
  - icons
Source: User Prompt
---

# Lucide React

## Definition
**Lucide React**는 **Lucide** 아이콘 라이브러리의 **React 전용 패키지**입니다. 1,500개 이상의 아름답고 일관된 오픈소스 SVG 아이콘을 React 컴포넌트 형태로 제공하며, Feather Icons의 포크 프로젝트로 시작해 큰 커뮤니티 성장을 이루었습니다. [Feather Icons](https://feathericons.com/)의 정신을 계승하되 더 활발히 유지보수되고 있습니다.

## Key Features (주요 특징)
- **Tree-shakable**: 사용한 아이콘만 번들에 포함되어 가볍습니다.
- **Inline SVG**: 각 아이콘이 React 컴포넌트로 렌더링되어 스타일링이 자유롭습니다.
- **Props Support**: `size`, `color`, `strokeWidth` 등 직관적인 Props를 지원합니다.
- **Accessibility**: ARIA 지원으로 웹 접근성이 우수합니다.
- **Lucide Lab**: 실험적이고 특수한 아이콘을 별도 패키지로 제공합니다.

## Installation
```bash
# npm
npm install lucide-react

# yarn
yarn add lucide-react

# pnpm
pnpm add lucide-react
```

## Basic Usage
```tsx
import { Camera, Heart, User } from 'lucide-react';

function App() {
  return (
    <div className="flex gap-6 p-8">
      {/* 기본 사용 */}
      <Camera size={32} color="blue" strokeWidth={1.5} />

      {/* 커스텀 스타일 (Tailwind CSS) */}
      <Heart className="text-red-500" size={48} fill="currentColor" />

      {/* 동적 아이콘 이름 (고급 옵션) */}
      <User size={24} absoluteStrokeWidth />
    </div>
  );
}
```

## Advanced Usage: Dynamic Icon Loading
이름으로 아이콘을 동적으로 불러올 때 유용합니다. (예: CMS 데이터 연동 등)

```tsx
import * as LucideIcons from 'lucide-react';

type IconName = keyof typeof LucideIcons;

interface IconProps {
  name: IconName;
  size?: number;
  color?: string;
}

function DynamicIcon({ name, size = 24, color }: IconProps) {
  const IconComponent = LucideIcons[name];
  if (!IconComponent) return null;
  return <IconComponent size={size} color={color} />;
}

// 사용 예시
<DynamicIcon name="AlertCircle" size={32} color="orange" />
```

## Props Reference
| Prop                  | Type    | Default      | Description                                   |
| --------------------- | ------- | ------------ | --------------------------------------------- |
| `size`                | number  | 24           | 아이콘 크기 (px)                              |
| `color`               | string  | currentColor | 색상 (CSS 색상 값)                            |
| `strokeWidth`         | number  | 2            | 선 두께                                       |
| `absoluteStrokeWidth` | boolean | false        | strokeWidth를 절대값으로 고정 (scale 시 유용) |
| `className`           | string  | -            | Tailwind 등 클래스 추가                       |

## Tips & Resources
- **Official Icons**: [Lucide Icons List](https://lucide.dev/icons) - 검색 및 미리보기 가능.
- **Framework Integration**:
    - **Next.js**: App Router에서 동적 import 시 Tree-shaking 최적화가 잘 동작합니다.
    - **Tailwind CSS**: `className`을 통해 `stroke`, `fill` 등을 완벽하게 제어할 수 있습니다.
- **Platforms**: React Native용 `lucide-react-native` 패키지도 존재합니다.

## Links
- [[React_MOC]]
