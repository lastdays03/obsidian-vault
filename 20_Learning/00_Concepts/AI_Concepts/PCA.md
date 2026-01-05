# PCA (Principal Component Analysis)

**Category**: #concept/ml/dimensionality-reduction
**Source**: [[54_ML_Unsupervised_Learning]]

## 정의
고차원 데이터를 **저차원으로 축소**하면서 **최대한 많은 정보를 보존**하는 기법입니다.

## 핵심 아이디어
데이터의 분산이 가장 큰 방향(주성분)을 찾아 새로운 축으로 사용합니다.

## 프로세스
1. 데이터 표준화
2. 공분산 행렬 계산
3. 고유값/고유벡터 계산
4. 상위 k개 주성분 선택

```python
from sklearn.decomposition import PCA

pca = PCA(n_components=2)  # 2차원으로 축소
X_reduced = pca.fit_transform(X)
```

## 활용
- 시각화 (3D → 2D)
- 노이즈 제거
- 연산 속도 향상
- 다중공선성 제거

## 단점
- 해석력 감소 (새로운 축은 원래 특성의 조합)

## 관련 개념
- [[Feature Engineering]]
- [[Silhouette Score]]
