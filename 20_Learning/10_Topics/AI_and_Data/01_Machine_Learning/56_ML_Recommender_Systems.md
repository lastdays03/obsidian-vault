---
Source: study_machine_learning
---

# 추천 시스템 기초 (Recommender Systems)
# 태그: #RecommenderSystem #CollaborativeFiltering #CosineSimilarity

이 문서는 `C2_M1` 주차의 노트북(`Exp_Recommender`)에서 다루어진 **추천 시스템**, 특히 **협업 필터링(Collaborative Filtering)**의 핵심 원리를 요약 정리한 것입니다.

---

## 1. 사용자 기반 협업 필터링 (User-Based CF)
가장 직관적이고 고전적인 추천 알고리즘입니다.
> "나와 취향이 비슷한(유사한) 사람이 좋아한 아이템이라면, 나도 좋아할 것이다."

### 1-1. 핵심 프로세스
1.  **User-Item Matrix 생성**: 사용자들의 아이템 평점 데이터를 행렬 형태로 만듭니다. (빈칸은 0 또는 결측치 처리)
2.  **유사도 계산**: 사용자 간의 평점 패턴이 얼마나 비슷한지 **코사인 유사도(Cosine Similarity)** 등을 이용해 계산합니다.
3.  **이웃 선정 (Neighbor Selection)**: 타겟 사용자와 유사도가 가장 높은 상위 N명의 사용자(Peer)를 찾습니다.
4.  **추천**: 이웃들은 봤지만(높은 평점), 타겟 사용자는 아직 안 본 아이템을 추천합니다.

### 1-2. 구현 예시 (Python)

```python
from sklearn.metrics.pairwise import cosine_similarity
import pandas as pd

# 1. 유사도 계산
# df: 행이 User, 열이 Item인 평점 행렬
user_sim = cosine_similarity(df)
user_sim_df = pd.DataFrame(user_sim, index=df.index, columns=df.index)

# 2. 가장 비슷한 유저 찾기 (자기 자신 제외)
target_user = 'User1'
similar_users = user_sim_df[target_user].sort_values(ascending=False)[1:]
top_peer = similar_users.index[0]

# 3. 추천 로직
# 나는 안 봤는데(0점), 친구는 본(0점 초과) 영화 필터링
my_ratings = df.loc[target_user]
peer_ratings = df.loc[top_peer]

recommendations = peer_ratings[(my_ratings == 0) & (peer_ratings > 0)]
print(f"추천 아이템: {recommendations.index.tolist()}")
```

---

## 2. 유사도 측정: 코사인 유사도 (Cosine Similarity)
두 벡터 사이의 각도를 이용하여 유사도를 측정합니다.
*   **값의 범위**: -1 ~ 1 (1에 가까울수록 매우 유사함)
*   **특징**: 평점의 절대적인 크기(1~5점)보다는 **방향성(패턴)**을 중시합니다. (예: 점수를 후하게 주는 사람과 짜게 주는 사람이라도 패턴이 같으면 유사하게 판단)

---

## 3. 인사이트 (Insights)
*   **Cold Start 문제**: 데이터가 전혀 없는 신규 가입자나 신규 아이템에 대해서는 유사도를 계산할 수 없어 추천이 불가능한 한계가 있습니다. (이를 보완하기 위해 '인기도 기반 추천' 등을 혼용)
*   **확장성**: 사용자가 수억 명으로 늘어나면 모든 사용자 간의 유사도를 실시간으로 계산하는 것이 불가능해지므로, **행렬 분해(Matrix Factorization)** 나 딥러닝 기반 모델로 발전하게 됩니다.
