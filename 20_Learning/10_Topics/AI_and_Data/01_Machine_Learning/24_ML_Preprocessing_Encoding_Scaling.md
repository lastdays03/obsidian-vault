---
Source: study_machine_learning
---

# 데이터 전처리 실습: 인코딩과 스케일링

## 1. 개요 및 목표
머신러닝 알고리즘이 데이터를 잘 이해할 수 있도록 돕는 핵심 전처리 기술을 실습합니다.
1.  **인코딩 (Encoding)**: 문자열(Category) 데이터를 숫자(Numerical)로 변환합니다.
2.  **스케일링 (Scaling)**: 서로 다른 단위(Scale)를 가진 변수들의 범위를 일정하게 맞춥니다.

## 1. 인코딩 (Encoding)

### 1-1. 레이블 인코딩 (Label Encoding)
*   카테고리형 데이터를 0, 1, 2... 와 같은 정수형으로 변환합니다.
*   **주의**: 숫자의 크기(순서)가 모델에 잘못된 영향을 줄 수 있습니다. (예: 냉장고(1) < 컴퓨터(2) ??)

```python
from sklearn.preprocessing import LabelEncoder

items = ['TV', '냉장고', '전자레인지', '컴퓨터', '선풍기', '선풍기', '믹서']

# LabelEncoder 객체 생성 및 학습
encoder = LabelEncoder()
encoder.fit(items)
labels = encoder.transform(items)

print('인코딩 변환값:', labels)
print('인코딩 클래스:', encoder.classes_)
# 디코딩 (역변환)
print('디코딩 원본값:', encoder.inverse_transform(labels))
```

### 1-2. 원-핫 인코딩 (One-Hot Encoding)
*   해당 클래스만 1, 나머지는 0으로 표현하는 희소 행렬(Sparse Matrix)을 만듭니다.
*   숫자의 크기 이슈를 해결할 수 있습니다.

```python
from sklearn.preprocessing import OneHotEncoder

# 2차원 데이터로 변환 (OneHotEncoder는 2D 입력을 기대함)
labels_2d = labels.reshape(-1, 1)

oh_encoder = OneHotEncoder()
oh_encoder.fit(labels_2d)
oh_labels = oh_encoder.transform(labels_2d)

print('원-핫 인코딩 데이터 (Sparse Matrix):\n', oh_labels)
print('\n원-핫 인코딩 데이터 (Array):\n', oh_labels.toarray())
```

**Pandas의 `get_dummies` 활용**: 사실 사이킷런보다 판다스가 더 간편할 때가 많습니다.
```python
import pandas as pd
df_items = pd.DataFrame({'item': items})
pd.get_dummies(df_items)
```

## 2. 피처 스케일링 (Feature Scaling)
데이터의 단위(Scale)가 다르면 모델 학습이 불안정해질 수 있습니다.

### 2-1. 표준화 (StandardScaler)
*   **평균을 0, 분산을 1**로 만듭니다. (정규분포화)
*   SVM, 로지스틱 회귀 등에서 필수적입니다.

```python
from sklearn.preprocessing import StandardScaler
from sklearn.datasets import load_iris

# 붓꽃 데이터 로드
iris = load_iris()
iris_df = pd.DataFrame(data=iris.data, columns=iris.feature_names)

scaler = StandardScaler()
# fit()과 transform()을 한 번에 수행
iris_scaled = scaler.fit_transform(iris_df)
iris_df_scaled = pd.DataFrame(data=iris_scaled, columns=iris.feature_names)

print('--- 스케일링 후 평균(Mean) ---')
print(iris_df_scaled.mean()) # 0에 근사
print('\n--- 스케일링 후 분산(Var) ---')
print(iris_df_scaled.var())  # 1에 근사
```

### 2-2. 정규화 (MinMaxScaler)
*   데이터를 **0과 1 사이**의 값으로 변환합니다.

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
iris_minmax = scaler.fit_transform(iris_df)
iris_df_minmax = pd.DataFrame(data=iris_minmax, columns=iris.feature_names)

print('최솟값(Min):')
print(iris_df_minmax.min()) # 0.0
print('\n최댓값(Max):')
print(iris_df_minmax.max()) # 1.0
```

### 💡 학습 데이터와 테스트 데이터의 스케일링 주의점
*   반드시 **학습 데이터(Train Set)로 `fit()`** 하고, 그 기준으로 **테스트 데이터(Test Set)를 `transform()`** 해야 합니다. (Data Leakage 방지)

## 3. 인사이트 도출 (Insights)

### 🌟 Key Takeaways
*   **인코딩의 선택**:
    *   **Label Encoding**: 간편하지만 순서 왜곡 위험. 트리 계열 외 주의.
    *   **One-Hot Encoding**: 범주 간 독립성 보장. 대부분의 모델에서 안전.
*   **스케일링의 필수성**: 단위(cm, kg 등)가 다르면 모델이 왜곡됨. `StandardScaler`나 `MinMaxScaler`로 공정 비교 환경 구성.
*   **Data Leakage 방지**: 스케일링 기준(`fit`)은 반드시 **Train Data**여야 함.

### 🔬 Try More
*   **변환 전후 비교**: 스케일링 여부에 따른 로지스틱 회귀/SVM 성능 차이 실험.
*   **RobustScaler**: 이상치에 강한 Scaler 사용해보기.
