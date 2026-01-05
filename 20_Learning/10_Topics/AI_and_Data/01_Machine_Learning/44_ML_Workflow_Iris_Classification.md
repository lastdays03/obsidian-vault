```
---
Source: study_machine_learning
---

---
Source: study_machine_learning
---

# 머신러닝 워크플로우 실습: 붓꽃(Iris) 품종 예측하기 (Hello World of ML)

## 1. 개요 및 목표
머신러닝의 가장 기본이 되는 **'분류(Classification)'** 프로세스를 처음부터 끝까지 실습합니다.
데이터를 로드하고, 모델을 학습시키고, 새로운 데이터의 정답을 예측하는 전체 흐름(Workflow)을 익히는 것이 핵심입니다.

### 프로세스 (Workflow)
1.  **준비 (Setup)**: 라이브러리 및 데이터 로드.
2.  **분리 (Split)**: 학습용 문제집과 시험용 문제집 나누기.
3.  **학습 (Train)**: 모델(학생)에게 학습용 문제집으로 공부시키기.
4.  **예측 (Predict)**: 시험용 문제집 풀게 하기.
5.  **평가 (Evaluate)**: 채점하기 (정확도).

## 1. 데이터 로드 및 확인
**Why**: 머신러닝의 재료는 데이터입니다. 데이터가 어떻게 생겼는지(Feature), 무엇을 맞춰야 하는지(Label) 확인해야 합니다.

```python
import pandas as pd
import sklearn
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

print(f"scikit-learn version: {sklearn.__version__}")
```
> **Output**: scikit-learn version: 1.8.0

```python
# 붓꽃 데이터 세트 로딩
iris = load_iris()
iris_data = iris.data
iris_label = iris.target

print('iris target값:', iris_label[:10])
print('iris target명:', iris.target_names)

# 보기 편하게 DataFrame으로 변환
df_iris = pd.DataFrame(data=iris_data, columns=iris.feature_names)
df_iris['label'] = iris.target
df_iris.head(3)
```
> **Analysis**: `load_iris()`를 통해 가져온 데이터는 `data`(Feature)와 `target`(Label)으로 구분됩니다. 이를 `DataFrame`으로 변환하여 가독성을 높이고 분석하기 쉬운 형태로 만듭니다.

## 2. 학습/테스트 데이터 분리 (Train/Test Split)
**Why**:
*   모든 데이터로 공부하고 그 데이터로 시험을 치면 100점이 나오겠지만, 실력을 검증할 수 없습니다. (**과적합, Overfitting** 방지)
*   따라서 공부할 데이터(Train)와 검증할 데이터(Test)를 분리해야 합니다.

```python
# test_size=0.2 : 전체 데이터의 20%를 테스트용으로 떼어놓음
# random_state=11 : 매번 실행할 때마다 똑같이 섞이도록 고정 (실습용)
X_train, X_test, y_train, y_test = train_test_split(iris_data, iris_label, test_size=0.2, random_state=11)

print(f"학습셋 크기: {X_train.shape}, 테스트셋 크기: {X_test.shape}")
```
> **Output**: 학습셋 크기: (120, 4), 테스트셋 크기: (30, 4)

## 3. 모델 학습 (Training)
**Why**: **의사결정트리(Decision Tree)** 알고리즘을 사용합니다. 스무고개와 비슷하게 "꽃잎 길이가 X보다 긴가?" 같은 질문을 반복하며 정답을 찾아가는 직관적인 모델입니다.

```python
# DecisionTreeClassifier 객체 생성
dt_clf = DecisionTreeClassifier(random_state=11)

# 학습 수행
dt_clf.fit(X_train, y_train)
```
> **Analysis**: `fit(X_train, y_train)` 메서드를 호출하여 모델이 데이터의 패턴을 학습하도록 합니다. 이 과정에서 모델은 Feature와 Label 사이의 규칙을 찾아냅니다.

## 4. 예측 수행 (Prediction)
**Why**: 학습된 모델이 보지 못한 데이터(Test Set)에 대해 어떤 답을 내놓는지 확인합니다.

```python
# 학습이 완료된 DecisionTreeClassifier 객체에서 테스트 데이터 세트로 예측 수행.
pred = dt_clf.predict(X_test)
```

## 5. 예측 정확도 평가 (Evaluation)
**Why**: 모델이 내놓은 답(`pred`)과 실제 정답(`y_test`)을 비교하여 점수를 매깁니다.

```python
from sklearn.metrics import accuracy_score
print('예측 정확도: {0:.4f}'.format(accuracy_score(y_test, pred)))
```
> **Analysis**: `accuracy_score`는 전체 테스트 데이터 중 올바르게 분류한 비율을 직관적으로 보여줍니다.

## 핵심 요약 (Insights)
1.  **전체 파이프라인**: 데이터 로드 -> 분할 -> 학습 -> 예측 -> 평가의 구조는 대부분의 머신러닝 작업에서 동일하게 적용됩니다.
2.  **데이터 분할의 중요성**: 학습 데이터와 테스트 데이터를 섞지 않는 것이 객관적인 모델 평가의 첫걸음입니다.
3.  **Scikit-learn 인터페이스**: `fit()`으로 학습하고, `predict()`로 예측하는 일관된 인터페이스를 제공하여 다양한 알고리즘을 쉽게 교체하여 사용할 수 있습니다.
