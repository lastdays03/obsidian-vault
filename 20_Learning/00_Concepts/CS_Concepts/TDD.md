# 개념 (Concept): TDD (Test Driven Development)

**태그**: #knowledge/concept #topic/CS #topic/Methodology
**출처**: [[04_Feature_Planner_Deep_Dive]]

---

## 📖 정의 (Definition)
**TDD(테스트 주도 개발)**는 실제 코드를 작성하기 전에 테스트 코드를 먼저 작성하는 소프트웨어 개발 방법론입니다. **"Red-Green-Refactor"**라는 짧은 주기를 반복하며 건강한 코드를 점진적으로 완성해 나갑니다.

---

## 💡 예시 (Example)
파이썬의 `unittest`를 사용한 TDD 사이클 예시입니다.

### 1단계: Red (실패하는 테스트 작성)
아직 구현되지 않은 `add` 함수를 테스트합니다. 실행하면 당연히 실패(Error)합니다.

```python
import unittest

# 아직 add 함수가 없음!

class TestCalculator(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(1, 2), 3)

if __name__ == '__main__':
    unittest.main()
# 결과: NameError: name 'add' is not defined (RED)
```

### 2단계: Green (테스트 통과를 위한 최소 구현)
테스트를 통과하기 위해 가장 빠르게(때로는 무식하게) 코드를 작성합니다.

```python
def add(a, b):
    return 3  # 죄악을 저질러서라도 일단 통과시킨다!

# 다시 테스트 실행 -> 통과 (GREEN)
```

### 3단계: Refactor (리팩토링)
중복을 제거하고 로직을 올바르게 수정합니다.

```python
def add(a, b):
    return a + b  # 일반적인 로직으로 개선

# 다시 테스트 실행 -> 여전히 통과 (GREEN 유지)
```

| **단계** | **의미** | **행동** |
| :--- | :--- | :--- |
| 🔴 **Red** | 실패 | 요구사항을 테스트 코드로 정의합니다. (컴파일 에러 포함) |
| 🟢 **Green** | 성공 | 테스트를 통과할 만큼만 최소한의 코드를 작성합니다. |
| 🔵 **Refactor** | 개선 | 코드의 구조와 가독성을 개선합니다. (기능 변경 없음) |

## 💡 Key Insights
- **설계 도구로서의 테스트**: TDD는 단순한 검증이 아니라, 코드를 어떻게 사용할지 먼서 고민하게 만드는 **설계(Design)** 과정입니다. 인터페이스를 먼저 정의하게 됩니다.
- **심리적 안정감**: 촘촘한 테스트 케이스는 리팩토링이나 기능 추가 시 기존 기능이 망가지지 않았다는 확신을 주어 개발자의 두려움을 없애줍니다.
- **오버헤드**: 초기 개발 속도가 느려 보일 수 있으나, 디버깅 시간과 유지보수 비용을 고려하면 장기적으로는 더 빠릅니다.
