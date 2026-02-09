# 개념 (Concept): TDD (Test Driven Development)

**태그**: #knowledge/concept #topic/Methodology #topic/Testing
**출처**: [[Robert_C_Martin]], [[Kent_Beck]], [[04_Feature_Planner_Deep_Dive]]

---

## 📖 정의 (Definition)
**TDD(Test-Driven Development)**는 실제 코드를 작성하기 전에 테스트 코드를 먼저 작성하고, 이를 통과하기 위한 최소한의 코드를 작성한 후 리팩토링하는 소프트웨어 개발 방법론입니다.
*핵심은 'Red(실패) - Green(성공) - Refactor(개선)'의 짧은 반복 주기를 통해 코드의 품질과 설계(Design)를 동시에 개선하는 것입니다.*

---

## 💡 예시 (Example)
*(Red-Green-Refactor Cycle)*

1.  **Red**: 실패하는 테스트 작성 (`add(1, 2) == 3`)
2.  **Green**: 테스트 통과를 위한 최소 구현 (`return 3`)
3.  **Refactor**: 중복 제거 및 로직 개선 (`return a + b`)

```python
import unittest

class TestCalculator(unittest.TestCase):
    def test_add(self):
        # 1. Red: 아직 add 함수가 없거나 구현되지 않음
        self.assertEqual(add(1, 2), 3)

def add(a, b):
    # 2. Green: 죄악을 저질러서라도 일단 통과 (Fake It)
    # return 3 
    
    # 3. Refactor: 올바른 로직으로 개선
    return a + b
```

---

## ⚖️ 비교 (Comparison)
| Feature | TDD (Test Driven) | BDD (Behavior Driven) | Traditional Testing |
| :--- | :--- | :--- | :--- |
| **Focus** | Unit Level Implementation | System Behavior (User Spec) | Verification (Last Step) |
| **Language** | Programming Code (Assert) | Natural Language (Gherkin) | Code or Manual |
| **Main Goal** | **Design** & Code Quality | Communication & Requirements | Bug Finding |
| **Creator** | Kent Beck | Dan North | - |

---

## 🔑 Key Insights
- **설계 도구 (Design Tool)**: TDD는 단순한 테스팅 기법이 아니라, **"어떻게 코드를 사용할 것인가?"**를 먼저 고민하게 만드는 설계 도구입니다. (Interface First)
- **심리적 안정감**: 촘촘한 테스트 케이스는 리팩토링이나 새로운 기능 추가 시 기존 기능이 망가지지 않았다는 확신(Confidence)을 줍니다.
- **YAGNI 실천**: 테스트를 통과할 만큼만 작성하므로, 오버 엔지니어링을 방지하고 필요한 코드만 작성하게 됩니다.

## 📚 References
- [Test Driven Development: By Example - Kent Beck](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)
- [The Clean Code Blog - TDD](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html)
