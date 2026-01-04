---
tags: [knowledge/concept, topic/CS, topic/Python]
Up: [[Learning_MOC]]
---

# 개념 (Concept): 동적 타이핑 (Dynamic Typing)

**출처**: [[01. python_variables_datatypes]]

---

## 📖 정의 (Definition)
**동적 타이핑(Dynamic Typing)**은 변수를 선언할 때 자료형을 지정하지 않고, 런타임에 값이 할당될 때 변수의 타입이 결정되는 프로그래밍 언어의 특성입니다.

---

## 💡 예시 (Example)
파이썬(Python)은 대표적인 동적 타이핑 언어입니다.

```python
x = 10      # x는 정수(int)
print(type(x))  # <class 'int'>

x = "Hello" # x는 이제 문자열(str) - 재할당 가능
print(type(x))  # <class 'str'>
```

반면, 자바(Java)나 C++ 같은 **정적 타이핑(Static Typing)** 언어는 컴파일 시점에 타입이 결정됩니다.

```java
// Java Examples
int x = 10;
// x = "Hello"; // Error: 컴파일 에러 발생
```

| **구분** | **설명** | **장점** | **단점** |
| :--- | :--- | :--- | :--- |
| **동적 타이핑** | 런타임에 타입 결정 | 코드 작성이 빠르고 유연함 | 실행 도중 타입 에러 발생 가능성 있음 |
| **정적 타이핑** | 컴파일 타임에 타입 결정 | 타입 안정성 높음, 최적화 유리 | 코드가 길어지고 유연성이 떨어짐 |

## 💡 Key Insights
- **유연성 vs 안정성**: 동적 타이핑은 빠른 프로토타이핑에 유리하지만, 대규모 프로젝트에서는 타입 오류를 방지하기 위해 **타입 힌트(Type Hints)** 등을 활용하여 정적 분석을 보완하는 추세입니다.
- **Duck Typing**: "오리처럼 걷고 오리처럼 꽥꽥거리면 오리다"라는 철학은 동적 타이핑 언어의 핵심으로, 객체의 실제 타입보다는 객체가 가진 메서드나 속성이 중요합니다.
