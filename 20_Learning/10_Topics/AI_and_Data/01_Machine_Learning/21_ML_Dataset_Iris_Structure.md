```
---
Source: study_machine_learning
---

# 데이터셋 분석: 붓꽃(Iris) 데이터 구조 탐색

## 1. 개요 및 목표
사이킷런(sklearn)에 내장된 예제 데이터셋인 **Iris 데이터셋**의 내부 구조를 뜯어봅니다.
머신러닝 데이터를 다룰 때 흔히 접하게 될 `Bunch` 객체(딕셔너리와 유사)의 형태에 익숙해지는 것이 목표입니다.

## 1. 데이터셋 키(Keys) 확인
`Bunch` 객체는 파이썬의 `dict`와 비슷합니다. 어떤 키값들이 있는지 확인해봅니다.
*   `data`: 입력 변수 (Feature)
*   `target`: 출력 변수 (Label, 정답)
*   `feature_names`: 입력 변수의 이름 (컬럼명)
*   `target_names`: 정답의 이름 (클래스명)

```python
from sklearn.datasets import load_iris

iris_data = load_iris()
print(f"iris_data type: {type(iris_data)}")

keys = iris_data.keys()
print(f"붓꽃 데이터 세트의 키들: {keys}")
```
> **Output**: 
> iris_data type: <class 'sklearn.utils._bunch.Bunch'>
> 붓꽃 데이터 세트의 키들: dict_keys(['data', 'target', 'frame', 'target_names', 'DESCR', 'feature_names', 'filename', 'data_module'])

## 2. Feature와 Target의 이름 확인
**Why**: 우리가 다루는 데이터가 무엇을 의미하는지(꽃받침 길이인지, 너비인지 등) 알아야 분석을 할 수 있습니다.

```python
print(f"feature_names: {iris_data.feature_names}")
print(f"target_names: {iris_data.target_names}")
```
> **Output**:
> feature_names: ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']
> target_names: ['setosa' 'versicolor' 'virginica']

## 3. 실제 데이터(Data)와 정답(Target) 확인
머신러닝 모델에는 **Numpy Array** 형태의 숫자가 입력됩니다.

```python
print(f"data type: {type(iris_data.data)}")
print(f"data shape: {iris_data.data.shape}") # (150개 샘플, 4개 특징)
print(f"target shape: {iris_data.target.shape}") # (150개 정답)

print("\n--- Data 예시 (5개) ---")
print(iris_data.data[:5])

print("\n--- Target 예시 ---")
print(iris_data.target)
```
> **Output**:
> data shape: (150, 4)
> target shape: (150,)

## 4. 인사이트 도출 (Insights)

### 🌟 Key Takeaways
*   **Bunch 객체**: 사이킷런 데이터셋은 딕셔너리와 유사한 `Bunch` 객체로 되어 있으며, `data`(문제지)와 `target`(정답지)이 분리되어 저장됩니다.
*   **데이터의 형태(Shape)**: `(150, 4)`라는 형태를 통해, 150개의 샘플과 4가지 특징(꽃받침 길이/너비, 꽃잎 길이/너비)이 있음을 직관적으로 이해했습니다.
*   **메타 데이터**: `feature_names`와 `target_names`를 통해 숫자로 된 데이터가 현실 세계에서 무엇을 의미하는지 연결할 수 있습니다.

### 🔬 Try More
*   **다른 데이터셋 탐색**: `load_diabetes()`나 `load_digits()`와 같은 다른 내장 데이터셋을 로드하고 구조를 비교해보세요.
*   **Pandas 변환**: `Bunch` 객체를 바로 `pd.DataFrame`으로 변환하여 `describe()`, `info()` 함수로 기초 통계량을 확인해보세요.
