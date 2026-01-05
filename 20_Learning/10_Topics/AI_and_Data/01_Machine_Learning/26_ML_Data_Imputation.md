---
Source: study_machine_learning
---

---
Source: study_machine_learning
---

# 데이터 전처리 실습: 결측치 처리 (Imputation)

## 1. 개요 및 목표
현실의 데이터는 완벽하지 않습니다. 빈 값(Null, NaN)을 어떻게 처리하느냐에 따라 머신러닝 모델의 성능이 크게 달라집니다.
사이킷런의 **`SimpleImputer`** 를 사용하여 결측치를 채우는 다양한 전략을 실습합니다.

## 1. 결측치 확인
먼저 데이터에 구멍이 얼마나 있는지 확인해야 합니다.

```python
import pandas as pd
import numpy as np

# 타이타닉 데이터 로드
train = pd.read_csv('https://bit.ly/fc-ml-titanic')

# 결측치 확인 (True/False)
print(train.isnull().sum())

# 특정 컬럼(Age)의 결측치 개수
print('Age 결측치 수:', train['Age'].isnull().sum())
```

## 2. 수치형 데이터 결측치 처리
`Age`와 같은 수치형 데이터는 평균(Mean)이나 중앙값(Median)으로 채우는 것이 일반적입니다.

### 2-1. Pandas `fillna()` 활용
간단한 경우 판다스만으로도 충분합니다.

```python
# 평균으로 채우기
train['Age'].fillna(train['Age'].mean()).describe()
```

### 2-2. Sklearn `SimpleImputer` 활용
파이프라인 구축이나 일관된 전처리를 위해서는 사이킷런의 Imputer가 유리합니다.
`strategy` 옵션을 통해 채우는 방식을 결정합니다.
*   `mean`: 평균
*   `median`: 중앙값
*   `most_frequent`: 최빈값 (범주형에도 사용 가능)
*   `constant`: 고정값 (예: 0, -1)

```python
from sklearn.impute import SimpleImputer

# 평균으로 채우는 Imputer 생성
imputer = SimpleImputer(strategy='mean')

# Age 컬럼에 적용 (2차원 배열 입력 필요)
age_values = train['Age'].to_numpy().reshape(-1, 1)

imputer.fit(age_values)
age_imputed = imputer.transform(age_values)

train['Age'] = age_imputed
print('Imputation 후 Age 결측치:', train['Age'].isnull().sum())
```

## 3. 범주형 데이터 결측치 처리
`Embarked`(탑승 항구)와 같은 문자열(범주형) 데이터는 `mean`을 구할 수 없습니다.
이 경우 **최빈값(`most_frequent`)** 으로 채우는 것이 일반적입니다.

```python
# Embarked 결측치 처리
imputer_cat = SimpleImputer(strategy='most_frequent')

# fit & transform
embarked_values = train['Embarked'].to_numpy().reshape(-1, 1)
embarked_imputed = imputer_cat.fit_transform(embarked_values)

train['Embarked'] = embarked_imputed
print('Imputation 후 Embarked 결측치:', train['Embarked'].isnull().sum())
```

## 4. 인사이트 도출 (Insights)
### 🌟 Key Takeaways
*   **Imputation의 필요성**: 대부분의 머신러닝 알고리즘은 결측치가 포함된 데이터를 입력받을 수 없습니다. (NaN 에러 발생)
*   **전략의 중요성**: 무조건 0이나 평균으로 채우는 것이 정답은 아닙니다. 데이터의 분포(Distribution)를 보고 결정해야 합니다. 이상치(Outlier)가 많다면 평균보다는 중앙값(Median)이 안전할 수 있습니다.
*   **Data Leakage 주의**: Imputer의 `fit`은 반드시 **Train Set**에 대해서만 수행해야 합니다. Test Set의 평균을 사용하여 Test Set을 채우면 안 됩니다.
