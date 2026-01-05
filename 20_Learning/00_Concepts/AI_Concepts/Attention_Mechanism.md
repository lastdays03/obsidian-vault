# Attention Mechanism (어텐션 메커니즘)

**Category**: #concept/dl/architecture
**Source**: [[31_DL_Attention_and_RAG]]

## 정의
입력 시퀀스에서 **중요한 부분에 집중**하도록 하는 메커니즘입니다.

## 핵심 아이디어
"모든 단어가 동일하게 중요하지 않다" - 사람이 글을 읽을 때 중요한 단어에 집중하는 것과 같습니다.

## Self-Attention
- **Query (Q)**: "나와 관련된 정보를 찾고 싶어"
- **Key (K)**: "나는 이런 정보를 가지고 있어"
- **Value (V)**: "나의 실제 정보는 이거야"

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

## 혁명적 영향
Transformer의 핵심 → BERT, GPT 등 초거대 모델의 기반

## 관련 개념
- [[Transformer]]
- [[RAG]]
