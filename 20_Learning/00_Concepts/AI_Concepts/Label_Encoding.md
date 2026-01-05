# Label Encoding (레이블 인코딩)

**Category**: #concept/ml/preprocessing
**Source**: [[24_ML_Preprocessing_Encoding_Scaling]]

## 정의
범주형 데이터를 **정수로 변환**하는 인코딩 기법입니다.

## 예시
```python
from sklearn.preprocessing import LabelEncoder

colors = ['red', 'blue', 'green', 'blue', 'red']
encoder = LabelEncoder()
encoded = encoder.fit_transform(colors)
# [2, 0, 1, 0, 2]
```

## 주의사항
- 순서가 없는 범주형 데이터에 사용 시 **모델이 순서를 오해**할 수 있음
- 예: 'red'=0, 'blue'=1, 'green'=2 → 모델이 green > blue > red로 인식

## 적합한 경우
- 순서가 있는 범주형 데이터 (예: '낮음', '중간', '높음')
- 타겟 변수 인코딩

## 관련 개념
- [[One-Hot Encoding]]
- [[Feature Engineering]]
