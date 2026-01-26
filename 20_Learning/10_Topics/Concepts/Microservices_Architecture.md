# 개념 (Concept): Microservices Architecture (MSA)

**태그**: #knowledge/concept #topic/Architecture
**출처**: User Request & 2025 Tech Trends

---

## 📖 정의 (Definition)
마이크로서비스 아키텍처(MSA)는 하나의 거대한 애플리케이션을 **작고 독립적인 서비스**들의 집합으로 나누어 구성하는 아키텍처 스타일입니다.
각 서비스는 **비즈니스 경계(Bounded Context)**를 중심으로 구축되며, **자신만의 데이터베이스**를 가지고 **독립적으로 개발, 배포, 스케일링**이 가능하다는 점이 핵심입니다.

> **비유**: 모노리스가 하나의 큰 아파트 건물이라면, 마이크로서비스는 여러 개의 독립적인 오피스텔이나 빌라들이 모인 단지와 같습니다.

---

## 🏗 전형적인 구성 요소 (2025~2026 표준 스택)

| 계층 | 대표 기술 스택 | 주요 역할 |
|:---|:---|:---|
| **API Gateway** | Kong, Traefik, Spring Cloud Gateway | 단일 진입점, 인증/인가, 라우팅, Rate Limiting |
| **서비스 간 통신** | gRPC (동기), Kafka / NATS (비동기) | 고성능 내부 통신 및 이벤트 기반 아키텍처(EDA) |
| **Service Discovery** | Kubernetes Service, Consul | 서비스 위치 자동 발견 및 로드 밸런싱 |
| **관측성 (Observability)** | OpenTelemetry, Prometheus, Grafana | 분산 추적(Tracing), 메트릭, 로그 통합 관리 |
| **설정 관리**| Vault, etcd, ConfigMap | 중앙화된 설정 및 보안 비밀 관리 |
| **CI/CD** | ArgoCD (GitOps), GitHub Actions | 독립적인 배포 파이프라인 자동화 |

---

## ⚖️ 장점과 단점 (Pros & Cons)

### 장점 (Why use it?)
1.  **독립적 스케일링**: 트래픽이 몰리는 특정 서비스(예: 주문, 결제)만 별도로 확장할 수 있어 비용 효율적입니다.
2.  **기술 다양성 (Polyglot)**: 서비스별로 최적의 언어와 DB를 선택할 수 있습니다 (예: AI는 Python, 결제는 Go).
3.  **장애 격리 (Fault Isolation)**: 특정 서비스에 장애가 발생해도 시스템 전체로 전파되지 않도록 설계할 수 있습니다 (Circuit Breaker 활용).
4.  **팀 자율성**: 소규모 팀(2-Pizza Team)이 서비스의 생명주기 전체를 책임지며 빠른 의사결정을 내릴 수 있습니다.

### 단점 (The Pain Points)
1.  **분산 시스템의 복잡도**: 네트워크 지연, 데이터 일관성(Eventual Consistency) 관리, 분산 트랜잭션 구현이 매우 어렵습니다.
2.  **운영 오버헤드**: 관리해야 할 서비스, DB, 로그, 모니터링 포인트가 기하급수적으로 늘어납니다.
3.  **데이터 무결성**: 서비스별 DB 사용으로 인해 조인(Join)이 불가능하며, **Saga 패턴**이나 **CQRS** 등의 복잡한 패턴이 강제됩니다.
4.  **초기 비용**: 서비스 간 통신 인프라와 배포 자동화를 구축하는 데 모노리스보다 훨씬 많은 시간이 소요됩니다.

---

## 🛠 2025~2026 주요 디자인 패턴
- **Saga Pattern**: 분산 트랜잭션 대신, 로컬 트랜잭션의 연속으로 비즈니스 로직을 처리하고 실패 시 보상 트랜잭션(Compensating Transaction)을 실행합니다.
- **CQRS**: 명령(Write)과 조회(Read)의 모델을 분리하여 읽기 성능을 최적화하고 복잡한 조회를 해결합니다.
- **Transactional Outbox**: DB 업데이트와 메시지 발행을 원자적으로 처리하여 데이터 유실을 방지합니다.
- **Strangler Fig Pattern**: 레거시 모노리스에서 기능을 하나씩 마이크로서비스로 전환할 때 사용하는 점진적 이주 패턴입니다.

---

## 🔑 Key Insights (2025 기준 판단 전략)
- **"조직 구조가 곧 소프트웨어 구조다 (Conway's Law)"**: MSA는 기술적 해결책이기 전에, 대규모 팀의 협업 병목을 해결하기 위한 **조직 관리 해결책**입니다.
- **Pragmatic MSA**: 2025년의 트렌드는 무조건적인 분리가 아닌, **모듈형 모노리스**로 시작하여 필요할 때만 서비스로 떼어내는 실용주의적 접근입니다.
- **Zero Trust Security**: 서비스 간 통신에서도 인증과 암호화(mTLS)를 필수로 적용하는 추세입니다.

---

## 📚 References
- User Request: Microservices Architecture 정리 (2025~2026 현실 기준)
- [Microservices.io](https://microservices.io/)
- 2025 Distributed Systems Trends Report
