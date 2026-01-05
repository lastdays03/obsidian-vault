---
tags: [concept/ui, tech/react]
Up: [[CS_Concepts_MOC]]
---

# React-in-Terminal

> **React-in-Terminal**은 웹 브라우저가 아닌 터미널(CLI) 환경에서 React의 선언적 UI 모델과 컴포넌트 시스템을 사용하여 사용자 인터페이스를 렌더링하는 기술입니다.

## 📖 Definition
전통적인 CLI 도구는 `printf`나 `cursers` 기반의 명령형(Imperative) 방식으로 UI를 그렸습니다. 반면 **React-in-Terminal**은 React의 **Reconciliation** 알고리즘을 터미널 문맥에 적용합니다.
주로 `ink` 라이브러리를 사용하며, Flexbox 레이아웃(`yoga-layout`)을 통해 터미널 창 크기 변화에 반응하는 **Responsive CLI**를 구축할 수 있습니다.

## 💻 Example
`ink` 라이브러리를 사용한 간단한 Counter 예제입니다.

```jsx
import React, { useState, useEffect } from 'react';
import { render, Text } from 'ink';

const Counter = () => {
    const [count, setCount] = useState(0);

    useEffect(() => {
        const timer = setInterval(() => {
            setCount(prev => prev + 1);
        }, 100);

        return () => clearInterval(timer);
    }, []);

    return <Text color="green">{count} tests passed</Text>;
};

render(<Counter />);
```

## 🆚 Comparison
| 특징 | Traditional CLI | React-in-Terminal |
| :--- | :--- | :--- |
| **패러다임** | 명령형 (Imperative) | 선언적 (Declarative) |
| **상태 관리** | 수동 관리 (Global Vars) | State / Effect Hook |
| **레이아웃** | 문자열 계산 (X, Y) | Flexbox (Yoga) |
| **복잡도** | 텍스트 출력에 유리 | 복잡한 UI/Interaction에 유리 |

## 🔗 Connected Concepts
- [[React]]
- [[Virtual DOM]]
- [[01_CLI_Foundation]]

Source: [[01_CLI_Foundation]]
