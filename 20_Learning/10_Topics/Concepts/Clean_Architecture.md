# 개념 (Concept): Clean Architecture

**태그**: #knowledge/concept #topic/Architecture
**출처**: [[Robert_C_Martin]], [[Clean_Architecture_Book]]

---

## 📖 정의 (Definition)
**Clean Architecture**는 Robert C. Martin(Uncle Bob)이 제안한 소프트웨어 구조 설계 패턴으로, **비즈니스 로직(Entities, Use Cases)을 프레임워크, UI, 데이터베이스와 같은 외부 요소로부터 철저히 분리**하여 시스템의 유지보수성과 테스트 용이성을 극대화하는 아키텍처입니다.
*핵심 원칙은 '의존성 규칙(Dependency Rule)'으로, 소스 코드 의존성은 반드시 안쪽(고수준 정책)을 향해야 하며, 안쪽 원은 바깥쪽 원에 대해 아무것도 몰라야 합니다.*

---

## 💡 예시 (Example)
*(전형적인 계층 구조 - The Onion)*

1.  **Frameworks & Drivers (Blue)**: DB, Web, UI, Devices (가장 바깥쪽)
2.  **Interface Adapters (Green)**: Controllers, Gateways, Presenters (데이터 변환)
3.  **Application Business Rules (Red)**: Use Cases (애플리케이션 특화 로직)
4.  **Enterprise Business Rules (Yellow)**: Entities (핵심 비즈니스 객체 - 가장 안쪽)

```python
# 의존성 방향 (Dependency Direction)
# Controller -> (implements) -> Use Case Input Port
# Use Case Interactor -> (calls) -> Presenter Interface
# Presenter -> (implements) -> Presenter Interface

# Bad: Business Logic importing Database models directly
from database.models import User # ❌ Violation

# Good: Business Logic defines an interface, Database implements it
class UserRepository(Protocol): # Interface defined in Inner Layer
    def save(self, user): ...

class SqlAlchemyUserRepo(UserRepository): # Implementation in Outer Layer
    ...
```

---

## ⚖️ 비교 (Comparison)
| Feature | Clean Architecture | Hexagonal (Ports & Adapters) | Onion Architecture | MVC (Model-View-Controller) |
| :--- | :--- | :--- | :--- | :--- |
| **Core Idea** | Dependency Rule (Layers) | Ports & Adapters | Concentric Layers | Separation of Presentation |
| **Focus** | Independence from DB/UI | Testability via Ports | Domain Model Centric | UI Interaction Flow |
| **Structure** | 4 Concentric Circles | Inside (Core) vs Outside | Core, Domain, Service, Infra | Model, View, Controller |
| **Relationship** | Superset / Clarification | Conceptual Grandparent | Conceptual Sibling | Component Pattern (compatible) |

---

## 🔑 Key Insights
- **프레임워크 독립성**: 아키텍처가 라이브러리의 존재 여부에 의존하지 않으므로, 도구를 그저 도구로 사용할 수 있습니다.
- **테스트 용이성 (Testability)**: UI, DB, Web Server 없이도 비즈니스 로직을 완벽하게 테스트할 수 있습니다.
- **UI/DB 독립성**: 비즈니스 로직을 건드리지 않고 UI를 웹에서 모바일로, DB를 Oracle에서 Mongo로 교체할 수 있습니다.
- **의존성 역전 원칙 (DIP)**: 구현 세부사항(Detail)이 정책(Policy)에 의존하게 만드는 것이 핵심입니다.

## 📚 References
- [The Clean Code Blog - Robert C. Martin](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [Clean Architecture: A Craftsman's Guide to Software Structure and Design](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164)
