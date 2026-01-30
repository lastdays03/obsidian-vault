---
tags:
  - knowledge/topic
  - tech_stack/ai_dev_tools
  - langchain
  - langgraph
Source: User Request
---

# LangGraph

## 1. 개요 (Definition)
**LangGraph**는 LangChain 생태계의 핵심 확장 라이브러리로, 단순한 선형 구조를 넘어 **순환(Cycles)**, **상태 관리(State Management)**, 그리고 **인간의 개입(Human-in-the-loop)**이 필요한 복잡한 AI 에이전트 워크플로우를 구축하기 위해 설계되었습니다.

> [!NOTE] Relation with LangChain
> LangGraph와 LangChain은 경쟁 관계가 아닌 **상호 보완적** 관계입니다.
> *   **LangChain**: 애플리케이션의 '부품(Component)'과 '직선형 경로(Path)'를 제공.
> *   **LangGraph**: 이를 연결하여 복잡한 로직을 수행하는 '설계도(Blueprint)'이자 '제어 장치' 역할.

## 2. 핵심 비교 (Advanced Comparison)

| 구분            | LangChain (LCEL 중심)                        | LangGraph                                             |
| :-------------- | :------------------------------------------- | :---------------------------------------------------- |
| **기본 구조**   | **선형(Linear/DAG)**: A → B → C 순서         | **그래프(Cyclic)**: 노드와 엣지를 통한 순환 구조      |
| **상태 관리**   | **제한적**: 주로 단순 대화 기록(Memory) 저장 | **강력함**: 전역 'State' 객체를 통한 정밀 상태 관리   |
| **워크플로우**  | **체인(Chain)**: 고정된 단계별 실행          | **상태 머신(State Machine)**: 상황에 따른 유연한 분기 |
| **안정성/복구** | **단발성**: 오류 시 처음부터 다시 시작       | **영속성**: 체크포인트를 통한 실패 지점 재개 가능     |
| **주요 용도**   | 단순 RAG, 챗봇, 요약 파이프라인              | 멀티 에이전트 협업, 복잡한 재시도 로직, 에이전트 앱   |

## 3. 주요 구성 요소 (Components)

1.  **StateGraph (상태 그래프)**: 워크플로우의 뼈대. 상태 스키마를 정의하고 노드와 엣지를 추가하여 로직을 구성합니다.
2.  **Nodes (노드)**: 실제 작업을 수행하는 함수. 현재 상태를 입력받아 작업을 수행하고 상태 업데이트를 반환합니다.
3.  **Edges (엣지)**: 노드 사이의 경로.
    *   **Conditional Edges**: 조건(LLM의 판단 등)에 따라 다음 경로를 동적으로 결정하는 분기점.
4.  **Checkpointers (체크포인터)**: SQLite, Postgres, Redis 등을 백엔드로 사용하여 실행 상태를 영구 저장. 결함 허용(Fault Tolerance)과 협업의 핵심입니다.

## 4. 상황별 선택 가이드 (Decision Guide)

### ✅ LangChain을 선택해야 할 때
*   **단순 RAG 시스템**: 문서를 읽고 질문에 답하는 직선형 프로세스.
*   **원샷(One-shot) 작업**: 번역, 요약 등 한 번의 요청으로 끝나는 기능.
*   **빠른 검증**: 아이디어를 빠르게 코드로 구현해보고 싶을 때 (Rapid Prototyping).

### ✅ LangGraph를 선택해야 할 때
*   **멀티 에이전트 시스템**: 검색/작성/검수 에이전트 간의 대화와 협업이 필요할 때.
*   **자기 반성(Self-Correction)**: "답변이 충분한가?"를 판단하고 부족하면 다시 수행하는 루프가 필요할 때.
*   **장기 실행 워크플로우**: 작업이 오래 걸리며, 중간에 멈췄다가(Persistence) 다시 재개해야 할 때.

## 5. 프로젝트 적용 제언 (Hybrid Strategy)

'수술 후 회복 관리 매니저' 프로젝트를 위한 하이브리드 전략입니다.

1.  **진단서 OCR 및 단순 프로파일링**:
    *   👉 **LangChain**: 빠른 부품 조립 및 단순 파이프라인 구현.
2.  **주간 리포트 생성 및 전문가 검수**:
    *   👉 **LangGraph (HITL)**: AI 초안 -> [일시 정지/사람 검수] -> 최종 배포.
3.  **맞춤 식단 피드백 루프**:
    *   👉 **LangGraph (Loop)**: 환자의 피드백에 따라 식단 생성 단계를 반복하는 순환 로직.

## 6. 참고 자료 (References)
* [[LangChain]]
* [[AI_Dev_Tools_MOC]]
