# Silhouette Score (실루엣 점수)

**Category**: #concept/ml/metrics
**Source**: [[54_ML_Unsupervised_Learning]]

## 정의
클러스터링 품질을 평가하는 지표로, 각 샘플이 **자신의 클러스터에 얼마나 잘 속하는지** 측정합니다.

## 계산
$$s = \frac{b - a}{\max(a, b)}$$

- $a$: 같은 클러스터 내 다른 샘플들과의 평균 거리
- $b$: 가장 가까운 다른 클러스터 샘플들과의 평균 거리

## 점수 범위
- **1.0**: 완벽한 클러스터링
- **0.5~1.0**: 좋은 클러스터링
- **0.0**: 클러스터 경계가 모호함
- **< 0.0**: 잘못된 클러스터에 할당됨

```python
from sklearn.metrics import silhouette_score

score = silhouette_score(X, labels)
```

## 관련 개념
- [[PCA]]
