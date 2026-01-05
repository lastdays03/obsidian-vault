---
Source: study_machine_learning
---

# 머신러닝 비지도 학습 (Unsupervised Learning)
# 태그: #MachineLearning #Unsupervised #Clustering #PCA #KMeans #DBSCAN

이 문서는 `extra_09_unsupervised_in_depth.ipynb` 노트북에서 다루어진 **비지도 학습(Unsupervised Learning)**의 핵심 기법인 차원 축소와 군집화를 요약 정리한 것입니다.
비지도 학습은 정답(Label)이 없는 데이터에서 숨겨진 패턴이나 구조를 찾아내는 학습 방법입니다.

---

## 1. 차원 축소 (Dimensionality Reduction) - PCA
데이터의 특성(Feature)이 너무 많으면 시각화가 어렵고, 모델 학습 속도가 느려지며 성능이 저하될 수 있습니다(차원의 저주). 차원 축소는 정보를 최대한 보존하면서 특성을 줄이는 기법입니다.

### PCA (Principal Component Analysis, 주성분 분석)
*   **원리**: 데이터의 분산(변동성)을 가장 잘 설명하는 새로운 축(주성분)을 찾아 데이터를 투영합니다.
*   **활용**: 데이터 시각화(2D/3D), 노이즈 제거, 특성 추출 등.

```python
from sklearn.decomposition import PCA
from sklearn.datasets import load_iris
import pandas as pd

# 데이터 로드
iris = load_iris()

# PCA 수행 (2차원으로 축소)
pca = PCA(n_components=2)
iris_pca = pca.fit_transform(iris.data)

print(f"변환된 데이터 쉐이프: {iris_pca.shape}")
# > (150, 2) : 4개 특성이 2개로 줄어듦
```

---

## 2. 군집화 (Clustering)
유사한 데이터끼리 그룹으로 묶어주는 작업입니다.

### 2.1 K-Means (K-평균 군집화)
데이터를 **거리 기반**으로 K개의 그룹으로 묶는 가장 대중적인 알고리즘입니다.

*   **동작 방식**:
    1.  K개의 중심점(Centroid)을 임의로 지정.
    2.  각 데이터를 가장 가까운 중심점에 할당.
    3.  할당된 데이터들의 평균 위치로 중심점 이동.
    4.  중심점이 변하지 않을 때까지 2~3 반복.
*   **단점**:
    *   K(클러스터 개수)를 미리 지정해야 함.
    *   거리 기반이므로 원형 클러스터가 아니면 잘 작동하지 않음.
    *   이상치(Outlier)에 민감함.

```python
from sklearn.cluster import KMeans

# 클러스터 개수 3개로 설정
kmeans = KMeans(n_clusters=3, init='k-means++', max_iter=300, random_state=0)
kmeans.fit(iris_df)

print(kmeans.labels_) # 각 데이터가 속한 클러스터 번호 (0, 1, 2)
```

### 2.2 DBSCAN (Density-Based Spatial Clustering)
**밀도(Density)** 기반의 군집화 알고리즘입니다. 데이터가 촘촘하게 모여있는 곳을 클러스터로 정의합니다.

*   **장점**:
    *   K를 미리 지정할 필요가 없음.
    *   기하학적인 복잡한 모양의 클러스터도 잘 찾아냄.
    *   어느 클러스터에도 속하지 않는 **노이즈(이상치)**를 효과적으로 감지함 (라벨이 -1로 표시됨).
*   **주요 파라미터**:
    *   `eps` (Epsilon): 이웃을 정의하는 거리 반경.
    *   `min_samples`: 핵심 포인트(Core Point)가 되기 위한 이웃의 최소 개수.

```python
from sklearn.cluster import DBSCAN

dbscan = DBSCAN(eps=0.6, min_samples=8, metric='euclidean')
dbscan_labels = dbscan.fit_predict(iris.data)
```

---

## 3. 군집화 평가 - 실루엣 분석 (Silhouette Analysis)
정답이 없는 군집화 결과를 어떻게 평가할까요? **실루엣 계수(Silhouette Coefficient)**를 사용합니다.

*   **의미**: 각 데이터가 **자기 클러스터 내 데이터와는 얼마나 가깝고(응집도), 다른 클러스터와는 얼마나 멀리 있는지(분리도)**를 나타내는 지표입니다.
*   **범위**: -1 ~ 1 사이.
    *   `1`: 잘 분류됨 (가까운건 가깝고, 먼건 멈).
    *   `0`: 클러스터 경계에 위치.
    *   `-1`: 잘못 분류됨.

```python
from sklearn.metrics import silhouette_score

score = silhouette_score(iris.data, kmeans.labels_)
print(f"실루엣 계수: {score:.3f}")
```

---

## 4. 인사이트 (Insights)

### 🌟 Key Takeaways
1.  **차원의 저주 해결**: 특성이 너무 많을 때는 PCA를 통해 주성분만 남기면 시각화가 가능해지고 학습 효율이 올라갑니다.
2.  **군집화 선택**:
    *   데이터 구조가 원형이고 개수를 짐작할 수 있다면 -> **K-Means**
    *   데이터 모양이 복잡하거나 이상치가 많고 클러스터 개수를 모른다면 -> **DBSCAN**
3.  **평가는 신중하게**: 비지도 학습은 절대적인 정답이 없으므로, 실루엣 점수와 시각화를 통해 결과를 다각도로 해석해야 합니다.
