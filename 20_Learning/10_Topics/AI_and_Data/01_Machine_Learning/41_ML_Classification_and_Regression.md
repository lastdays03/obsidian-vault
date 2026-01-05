---
Source: study_machine_learning
---

# 머신러닝 모델: 분류와 회귀 (Classification & Regression)
# 태그: #MachineLearning #Classification #Regression #ScikitLearn

이 문서는 `extra_06_classification_in_depth.ipynb`와 `extra_07_regression_in_depth.ipynb` 노트북에서 다루어진 머신러닝의 두 가지 주요 지도 학습 유형인 **분류(Classification)**와 **회귀(Regression)**의 핵심 알고리즘 및 평가지표를 요약 정리한 것입니다.

---

## 1. 분류 (Classification)

분류는 입력 데이터를 정해진 카테고리(클래스) 중 하나로 예측하는 작업입니다.

### 핵심 알고리즘

#### 1.1 Logistic Regression (로지스틱 회귀)
이름은 '회귀'지만 실제로는 **분류** 알고리즘입니다. 선형 방정식을 사용해 학습하고, 시그모이드(Sigmoid) 함수를 통과시켜 0과 1 사이의 확률값으로 변환하여 클래스를 결정합니다.

*   **주요 특징**:
    *   이진 분류(Binary Classification)에 특화되어 있지만, OvR(One-vs-Rest) 방식을 통해 다중 분류도 지원합니다.
    *   희소한 데이터 세트나 매우 큰 데이터 세트에서도 잘 작동합니다.
*   **주요 파라미터 (`sklearn.linear_model.LogisticRegression`)**:
    *   `penalty`: 규제(Regularization) 유형을 설정합니다.
        *   `'l2'` (기본값): 릿지(Ridge) 방식. 일반적으로 가장 많이 사용됩니다.
        *   `'l1'`: 라쏘(Lasso) 방식. 불필요한 특성을 0으로 만듭니다.
        *   `'elasticnet'`: L1과 L2의 결합.
        *   `'none'`: 규제 미적용.
    *   `C`: 규제 강도의 역수입니다. (기본값 1.0)
        *   값이 **작을수록** 규제가 **강해집니다** (계수를 0에 가깝게 억제).
        *   값이 **클수록** 규제가 **약해집니다** (과적합 위험 증가).
    *   `solver`: 최적화에 사용할 알고리즘입니다. (`'lbfgs'`, `'liblinear'`, `'newton-cg'`, `'sag'`, `'saga'`)
        *   작은 데이터셋에는 `'liblinear'`가 적합할 수 있습니다.
    *   `max_iter`: 수렴을 위한 최대 반복 횟수입니다.

#### 1.2 K-Nearest Neighbors (K-최근접 이웃, KNN)
새로운 데이터가 주어졌을 때, 가장 가까운 K개의 이웃 데이터가 속한 클래스로 예측하는 직관적인 알고리즘입니다.

*   **주요 특징**:
    *   학습 과정이 별도로 필요 없으며(Lazy Learner), 예측 시점에 거리를 계산합니다.
    *   데이터 스케일에 민감하므로 전처리(Scaling)가 필수적입니다.
*   **주요 파라미터 (`sklearn.neighbors.KNeighborsClassifier`)**:
    *   `n_neighbors`: 참조할 이웃의 개수 (K). (기본값 5)
        *   K가 작으면: 모델이 복잡해지고, 노이즈에 민감해짐 (과적합 위험).
        *   K가 크면: 모델이 단순해지고, 결정 경계가 부드러워짐 (과소적합 위험).
    *   `weights`: 예측 시 이웃의 영향력 가중치.
        *   `'uniform'` (기본값): 모든 이웃에게 동일한 가중치.
        *   `'distance'`: 거리가 가까운 이웃에게 더 큰 가중치 부여.
    *   `metric`: 거리 측정 방식 (예: `'minkowski'`, `'euclidean'`).

---

### 분류 평가지표 (Evaluation Metrics)

*   **Accuracy (정확도)**: 전체 데이터 중 올바르게 예측한 비율. 데이터 불균형 시 신뢰하기 어렵습니다.
*   **Precision (정밀도)**: 양성(Positive)으로 예측한 것 중 실제 양성인 비율. (스팸 메일 분류 등에서 중요)
*   **Recall (재현율)**: 실제 양성 중 올바르게 양성으로 예측한 비율. (암 진단, 사기 탐지 등에서 중요)
*   **F1 Score**: Precision과 Recall의 조화 평균. 두 지표의 균형을 평가할 때 사용합니다.
*   **ROC-AUC**: 다양한 임계값(Threshold)에서의 모델 성능을 종합적으로 평가하는 지표. 1에 가까울수록 좋습니다.
*   **Confusion Matrix (오차 행렬)**: 예측 결과(TP, TN, FP, FN)를 4분면 행렬로 시각화하여 세부적인 오류 유형을 파악합니다.

---

## 2. 회귀 (Regression)

회귀는 입력 데이터를 바탕으로 연속적인 숫자(수치)를 예측하는 작업입니다.

### 핵심 알고리즘

#### 2.1 Linear Regression (선형 회귀)
입력 특성(Feature)들의 가중치 합(선형 결합)으로 타겟 값을 예측하는 가장 기본적인 모델입니다.

*   **주요 파라미터 (`sklearn.linear_model.LinearRegression`)**:
    *   `fit_intercept`: 절편(Bias)을 계산할지 여부 (기본값 `True`).
    *   `n_jobs`: 병렬 처리에 사용할 CPU 코어 수 (`-1`이면 모든 코어 사용).

#### 2.2 Ridge (릿지 회귀) - L2 규제
선형 회귀에 **L2 규제(Regularization)**를 적용한 모델입니다.

*   **특징**:
    *   모든 계수(가중치)를 0에 가깝게 만들지만, 완전히 0으로 만들지는 않습니다.
    *   특성 간 상관관계가 높을 때(다중공선성) 효과적입니다.
*   **주요 파라미터**:
    *   `alpha`: 규제 강도. 값이 클수록 규제가 강해져 계수 크기가 줄어들고 모델이 단순해집니다.

#### 2.3 Lasso (라쏘 회귀) - L1 규제
선형 회귀에 **L1 규제**를 적용한 모델입니다.

*   **특징**:
    *   불필요한 특성의 계수를 **완전히 0**으로 만들 수 있습니다.
    *   자동으로 **특성 선택(Feature Selection)** 효과를 가집니다.
*   **주요 파라미터**:
    *   `alpha`: 규제 강도. (노트북에서는 0.001 ~ 100까지 변화시키며 실험)

#### 2.4 ElasticNet (엘라스틱넷)
L1 규제와 L2 규제를 결합한 모델입니다. 릿지와 라쏘의 절충안으로 볼 수 있습니다.

---

### 회귀 평가지표 (Evaluation Metrics)

*   **MSE (Mean Squared Error)**: 오차(예측값 - 실제값)의 제곱의 평균.
    *   이상치(Outlier)에 민감하게 반응합니다(페널티가 큼).
*   **RMSE (Root Mean Squared Error)**: MSE에 루트를 씌운 값.
    *   실제 값과 단위가 같아져 해석하기 용이합니다.
*   **MAE (Mean Absolute Error)**: 오차의 절대값의 평균.
    *   이상치에 덜 민감합니다.
*   **R2 Score (결정 계수)**: 모델이 데이터의 분산을 얼마나 설명하는지 나타내는 지표.
    *   1에 가까울수록 예측 성능이 좋고, 0에 가까울수록 성능이 나쁨(평균 예측 수준)을 의미합니다.
