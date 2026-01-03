# 개념 (Concept): 상속 (Inheritance)

**태그**: #knowledge/concept #topic/CS #topic/OOP
**출처**: [[07. python_classes]]

---

## 📖 정의 (Definition)
**상속(Inheritance)**은 객체 지향 프로그래밍(OOP)에서 기존 클래스(부모/슈퍼 클래스)의 속성과 메서드를 새로운 클래스(자식/서브 클래스)가 물려받아 재사용하고 확장하는 메커니즘입니다.

---

## 💡 예시 (Example)
파이썬에서는 `class Child(Parent):` 문법을 사용합니다.

```python
class Animal:  # 부모 클래스
    def speak(self):
        print("동물이 소리를 냅니다")

class Dog(Animal):  # 자식 클래스 (Animal 상속)
    def bark(self):
        print("멍멍!")

dog = Dog()
dog.speak()  # "동물이 소리를 냅니다" (물려받은 메서드)
dog.bark()   # "멍멍!" (자신의 메서드)
```

| **용어** | **설명** |
| :--- | :--- |
| **Parent / Super / Base Class** | 상속을 해주는 클래스 (기반이 되는 클래스) |
| **Child / Sub / Derived Class** | 상속을 받는 클래스 (확장된 클래스) |
| **Overriding** | 상속받은 메서드를 자식 클래스에서 재정의하는 것 |

## 💡 Key Insights
- **코드 재사용성**: 공통 기능을 부모 클래스에 한 번만 작성하고 여러 자식 클래스에서 사용하여 중복을 줄입니다.
- **계층 구조**: `IS-A` 관계(예: 개는 동물이다)를 표현하여 시스템의 구조를 논리적으로 만듭니다.
- **다중 상속**: 파이썬은 여러 부모로부터 상속받는 다중 상속을 지원하며, MRO(Method Resolution Order)를 통해 충돌을 해결합니다.
