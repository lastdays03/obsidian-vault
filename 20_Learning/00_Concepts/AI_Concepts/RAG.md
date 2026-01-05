# RAG (Retrieval-Augmented Generation)

**Category**: #concept/dl/application
**Source**: [[31_DL_Attention_and_RAG]]

## 정의
LLM의 **환각(Hallucination) 문제**를 해결하기 위해 외부 지식을 검색하여 답변을 생성하는 기술입니다.

## 프로세스
1. **Retrieval (검색)**: 질문과 관련된 문서를 Vector DB에서 검색
2. **Augmentation (증강)**: 검색된 문서를 프롬프트에 추가
3. **Generation (생성)**: LLM이 참고 자료 기반으로 답변 생성

## 비유
"시험 칠 때 전공 서적을 펴놓고(Open Book) 답을 쓰는 것"

## 장점
- 정확성: 근거 기반 답변
- 최신성: 모델 재학습 없이 DB 업데이트만으로 반영

## 관련 개념
- [[Attention Mechanism]]
- [[Transformer]]
