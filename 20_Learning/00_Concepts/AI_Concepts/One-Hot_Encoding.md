# One-Hot Encoding (원-핫 인코딩)

**Category**: #concept/ml/preprocessing
**Source**: [[24_ML_Preprocessing_Encoding_Scaling]]

## 정의
범주형 데이터를 **이진 벡터**로 변환하는 인코딩 기법입니다.

## 예시
```python
from sklearn.preprocessing import OneHotEncoder

colors = [['red'], ['blue'], ['green']]
encoder = OneHotEncoder(sparse_output=False)
encoded = encoder.fit_transform(colors)
# [[0, 0, 1],  # red
#  [1, 0, 0],  # blue
#  [0, 1, 0]]  # green
```

## 특징
- 각 범주가 독립적인 특성으로 표현됨
- 순서 정보가 없어 모델이 오해하지 않음

## 단점
- **차원 증가**: 범주가 많으면 특성 수가 급증 (Curse of Dimensionality)

## 관련 개념
- [[Label Encoding]]
- [[Feature Engineering]]
