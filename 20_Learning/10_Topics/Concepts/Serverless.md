# 개념 (Concept): Serverless Architecture (서버리스)

**태그**: #knowledge/concept #topic/Architecture #topic/Cloud
**출처**: User Request & 2026 Tech Trends
**업데이트**: 2026-01-26

---

## 📖 정의 (Definition)
서버리스(Serverless)는 개발자가 서버 프로비저닝, 패치, 스케일링, 용량 계획 등을 전혀 신경 쓰지 않아도 되는 클라우드 실행 모델입니다. **"서버가 없다"는 뜻이 아니라**, 서버 관리에 대한 모든 책임을 클라우드 제공업체가 전담하며, 개발자는 오직 비즈니스 로직(코드)에만 집중합니다.

### 🛠 대표 제품 (2025~2026)
- **FaaS**: AWS Lambda, Google Cloud Functions, Azure Functions, Cloudflare Workers
- **CaaS (Serverless Container)**: AWS Fargate, Google Cloud Run
- **Edge/Frontend**: Vercel Functions, Netlify Functions

---

## 🏗 핵심 특징 (Key Features)

| 특징 | 설명 |
| :--- | :--- |
| **이벤트 기반 실행** | HTTP 요청, 메시지 큐, 파일 업로드, Cron 스케줄 등으로 트리거 |
| **자동 스케일링** | 0에서 수천 개의 인스턴스까지 초 단위로 자동 확장 및 축소 |
| **실행 기반 과금** | 실행 시간(ms), 호출 횟수, 데이터 전송량에 대해서만 비용 지불 (Idle 시 0원) |
| **제로 서버 관리** | OS 패치, 런타임 업데이트, 로드밸런싱 등을 클라우드가 완전 담당 |
| **실행 시간 최적화** | 주로 짧은 작업(1초~15분)에 최적화된 Stateless 환경 |

---

## ⚖️ 장점과 단점 (Pros & Cons)

### 장점 (Why use it?)
1.  **비용 효율성**: 간헐적/버스트 트래픽 환경에서 **70~95% 비용 절감** 가능.
2.  **운영 부담 제로**: 인프라 엔지니어 없이도 대규모 트래픽 처리가 가능한 확장성 제공.
3.  **초고속 배포**: 함수 단위 수정 및 배포로 실험과 롤아웃 속도가 극대화됩니다.
4.  **AI/ML 최적**: 이미지 처리, LLM 추론(Inference), 배치 작업에 매우 적합 (GPU 지원 확대 중).

### 단점 (The Pain Points)
1.  **콜드 스타트 (Cold Start)**: 첫 호출 시 발생하는 초기화 지연(100ms~15초).
2.  **벤더 락인 (Vendor Lock-in)**: 플랫폼 간 이벤트 모델과 API가 달라 이전이 어렵습니다.
3.  **관측성 난이도**: 분산된 로그와 실행 추적을 위해 **OpenTelemetry, X-Ray** 등이 필수적입니다.
4.  **비용 폭탄 위험**: 무한 루프나 DDoS 공격 시 호출 횟수 폭증에 대비한 Budget Alert가 필요합니다.
5.  **상태 관리**: 기본적으로 Stateless이므로 전역 상태를 위해 외부 DB/캐시 연결이 강제됩니다.

---

## 🚀 실무 적합 워크로드 Top 10

1.  **웹훅 / API 백엔드**: Vercel, Cloudflare Workers 기반의 경량 API
2.  **미디어 처리**: 이미지 리사이징, 비디오 변환 트리거
3.  **실시간 데이터 처리**: IoT 신호 처리, 로그 분석
4.  **AI/LLM 추론**: 챗봇 백엔드, RAG(Retrieval-Augmented Generation) 파이프라인
5.  **배치/크론잡**: 일일 보고서 생성, DB 정리 작업
6.  **알림 서비스**: 알림톡, 이메일, SMS 발송 처리
7.  **모바일 백엔드**: 인증 처리 및 푸시 알림
8.  **ETL 파이프라인**: 데이터 변환 및 적재의 일부 단계
9.  **A/B 테스트**: 엣지에서의 퍼스널라이제이션 룰 엔진
10. **프로토타이핑**: MVP 개발 및 스파이크 프로젝트

> [!WARNING]
> **부적합한 경우**: 항상 켜져 있어야 하는 게임 서버, 초저지연(10ms 이하) 금융 트레이딩, 30분 이상의 장기 실행 작업.

---

## 🔑 Key Insights
- **"운영 지옥의 탈출구"**: 서버리스는 기술적 스케일보다 **비용과 운영 생산성**에 초점을 맞춘 선택입니다.
- **하이브리드 전략**: 핵심 로직은 모노리스/MSA로 유지하되, 배치나 이벤트성 작업만 서버리스로 떼어내는 것이 2026년의 주류 패턴입니다.

---

## 📚 References
- [[Architecture_Comparison_Guide]]
- [[Monolithic_Architecture]]
- [[Microservices_Architecture]]
- [[FaaS]]
