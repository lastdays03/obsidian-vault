# Imputation (결측치 처리)

**Category**: #concept/ml/preprocessing
**Source**: [[26_ML_Data_Imputation]]

## 정의
데이터의 **결측값(Missing Value)**을 채우는 기법입니다.

## 주요 방법

### Simple Imputation
- **평균값**: 수치형 데이터, 정규분포 가정
- **중앙값**: 이상치가 있을 때
- **최빈값**: 범주형 데이터

### Advanced Imputation
- **KNN Imputation**: 유사한 샘플의 값으로 채움
- **Iterative Imputation**: 다른 특성으로 예측하여 채움

```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy='mean')
X_filled = imputer.fit_transform(X)
```

## 주의사항
- 결측 패턴 분석 먼저 (MCAR, MAR, MNAR)
- 무작정 채우면 편향 발생 가능

## 관련 개념
- [[Data Leakage]]
- [[Feature Engineering]]
