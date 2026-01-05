---
Source: study_machine_learning
---

# 딥러닝 심화: Attention 메커니즘과 RAG
# 태그: #DeepLearning #Attention #Transformer #RAG #LLM

이 문서는 `C2_M3` 주차의 노트북들(`Exp_Attention_Viz`, `Project_RAG_Chatbot`)에서 다루어진 최신 딥러닝 및 NLP(자연어 처리)의 핵심 기술인 **Attention**과 **RAG**를 요약 정리한 것입니다.

---

## 1. 어텐션(Attention) 메커니즘
"입력 시퀀스의 모든 부분이 동일하게 중요하지 않다"는 아이디어에서 출발했습니다. 사람이 글을 읽을 때 중요한 단어에 집중하는 것과 같습니다.

### 1-1. Self-Attention (자기 주의)
입력 문장 내의 단어들끼리 서로 어떤 관계가 있는지(얼마나 연관되어 있는지)를 계산합니다. Transformer 모델의 핵심입니다.

*   **Query (Q), Key (K), Value (V)**:
    *   **Query**: "나(현재 단어)와 관련된 정보를 찾고 싶어."
    *   **Key**: "나는 이런 정보를 가지고 있어." (Index 역할)
    *   **Value**: "나의 실제 정보는 이거야."
*   **작동 원리**:
    1.  Q와 K의 유사도(내적)를 계산하여 **Attention Score**를 구합니다.
    2.  Score를 Softmax에 통과시켜 확률값(가중치)으로 만듭니다. (**Attention Map**)
    3.  이 가중치를 V에 곱해서 더합니다(Weighted Sum).
    4.  결과적으로, 문맥상 중요한 단어의 정보가 더 많이 반영됩니다.

```python
import torch.nn.functional as F

# Attention 공식: softmax(Q @ K^T / sqrt(d_k)) @ V
scores = torch.matmul(query, key.transpose(-2, -1)) / math.sqrt(d_k)
p_attn = F.softmax(scores, dim=-1) # 확률값으로 변환
output = torch.matmul(p_attn, value)
```

---

## 2. RAG (Retrieval-Augmented Generation, 검색 증강 생성)
LLM(거대 언어 모델)의 환각(Hallucination) 문제를 해결하고, 최신 정보나 비공개 데이터를 활용하기 위한 기술입니다. "시험 칠 때 전공 서적을 펴놓고(Open Book) 답을 쓰는 것"과 비슷합니다.

### 2-1. 핵심 프로세스
1.  **Retrieval (검색)**: 사용자의 질문과 관련된 문서를 지식 베이스(Vector DB)에서 찾아옵니다.
    *   문서와 질문을 벡터로 변환(Embedding)하여 유사도를 계산합니다.
2.  **Augmentation (증강)**: 찾은 문서를 프롬프트에 "참고 자료"로 넣어줍니다.
3.  **Generation (생성)**: LLM이 참고 자료를 바탕으로 정확한 답변을 생성합니다.

### 2-2. 장점
*   **정확성**: 근거(Source)에 기반한 답변을 생성하므로 신뢰도가 높습니다.
*   **최신성**: 모델을 재학습하지 않아도, DB에 새로운 문서를 추가하면 바로 반영됩니다.

```python
# 예시: 간단한 코사인 유사도 기반 검색
from sklearn.metrics.pairwise import cosine_similarity

# 사용자 질문과 지식 베이스의 유사도 계산
similarities = cosine_similarity(user_query_vector, knowledge_base_vectors)
best_doc_index = np.argmax(similarities)

print(f"참고 문서: {knowledge_base[best_doc_index]}")
# > Retrieved: MLflow is a tool for managing the ML lifecycle.
```

---

## 3. 인사이트 (Insights)
*   **Transformer의 혁명**: Attention 메커니즘 덕분에 모델은 문맥을 훨씬 더 깊이 있게 이해할 수 있게 되었고, 이는 **BERT**, **GPT**와 같은 초거대 모델의 탄생으로 이어졌습니다.
*   **RAG의 실용성**: 기업용 AI 챗봇이나 전문 지식 Q&A 시스템을 구축할 때, 파인튜닝(Fine-tuning)보다 비용 대비 효과가 뛰어난 RAG가 표준 아키텍처로 자리 잡고 있습니다.
