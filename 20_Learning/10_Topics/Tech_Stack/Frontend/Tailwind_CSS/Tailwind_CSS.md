---
created: 2026-01-26
tags:
  - knowledge/topic
  - tech_stack/frontend
  - tech_stack/css
  - framework
  - utility_first
Source: User Prompt
---

# Tailwind CSS

## Definition
**Tailwind CSS**는 **Utility-First** 방식의 CSS 프레임워크로, 미리 정의된 유틸리티 클래스(예: `flex`, `pt-4`, `text-center`)를 조합하여 빠르고 일관된 디자인을 구축합니다. 2017년 공개 이후 폭발적으로 성장하여, 2026년 현재 Next.js 등 모던 웹 개발 스택의 **사실상 표준 스타일링 도구**가 되었습니다.

## Key Features (핵심 특징 - 2026년 기준)
- **Utility-First**: 별도의 CSS 파일 작성 없이 HTML 내에서 스타일을 완결하여 생산성과 유지보수성을 극대화합니다.
- **JIT (Just-In-Time) Compilation**: 사용된 클래스만 실시간으로 생성하여 빌드 속도가 빠르고 번들 크기가 매우 작습니다. (Tailwind v4는 Rust 엔진 Oxide 탑재로 10배 더 빠름)
- **Design System Friendly**: `tailwind.config.js`를 통해 프로젝트만의 색상, 폰트, 간격 등을 손쉽게 커스터마이징할 수 있습니다.
- **Arbitrary Values**: `w-[300px]`, `bg-[#1da1f2]` 처럼 대괄호 문법으로 임의의 값을 즉석에서 적용할 수 있습니다.
- **Best Practices Built-in**: 반응형 디자인(`md:`, `lg:`), 다크 모드(`dark:`), 상태 스타일(`hover:`, `focus:`)을 접두사만으로 간단히 구현합니다.

## Installation (Next.js 16)
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```
전역 CSS 파일(`globals.css`)에 디렉티브 추가:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Usage Example
```tsx
export default function Card() {
  return (
    <div className="max-w-sm rounded overflow-hidden shadow-lg bg-white dark:bg-gray-800">
      <div className="px-6 py-4">
        <div className="font-bold text-xl mb-2 text-gray-900 dark:text-white">
          Card Title
        </div>
        <p className="text-gray-700 dark:text-gray-300 text-base">
          Lorem ipsum dolor sit amet...
        </p>
      </div>
      <div className="px-6 pt-4 pb-2">
        <span className="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">#photography</span>
      </div>
    </div>
  );
}
```

## Comparison (2026 Trend)
| Feature         | Tailwind CSS       | CSS-in-JS (Styled-components) | CSS Modules             |
| --------------- | ------------------ | ----------------------------- | ----------------------- |
| **Speed**       | Very Fast          | Slow (Runtime overhead)       | Fast                    |
| **File Size**   | Tiny (Purged)      | Medium                        | Small                   |
| **Maintenance** | High (Co-location) | Medium                        | Medium (File switching) |
| **Trend**       | Dominant           | Declining                     | Stable                  |

## Links
- [[Frontend_MOC]]
- [[Nextjs]] (Best combination)
