---
Source: study_machine_learning
---

# 머신러닝 앙상블 학습 (Ensemble Learning)
# 태그: #MachineLearning #Ensemble #RandomForest #XGBoost #LightGBM

이 문서는 `extra_08_ensemble_in_depth.ipynb` 노트북에서 다루어진 **앙상블 학습(Ensemble Learning)**의 핵심 기법과 알고리즘을 요약 정리한 것입니다.

---

## 1. 앙상블 학습 개요
앙상블 학습은 여러 개의 머신러닝 모델(Weak Learner)을 결합하여, 단일 모델보다 더 강력한 예측 성능(Strong Learner)을 이끌어내는 방법입니다.
> "백지장도 맞들면 낫다." (집단 지성)

### 주요 유형
1.  **보팅 (Voting)**: 서로 다른 알고리즘을 가진 모델들이 투표를 통해 최종 예측을 결정.
2.  **배깅 (Bagging)**: 같은 알고리즘으로 다른 데이터 샘플(부트스트래핑)을 학습해 결합. (예: 랜덤 포레스트)
3.  **부스팅 (Boosting)**: 이전 모델이 틀린 오차를 다음 모델이 순차적으로 학습해 개선. (예: GBM, XGBoost, LightGBM)
4.  **스태킹 (Stacking)**: 여러 모델의 예측값을 다시 학습 데이터로 사용하여 최종 메타 모델이 예측.

---

## 2. 보팅 (Voting)
여러 모델의 의견을 통합하는 가장 기초적인 앙상블 기법입니다.

*   **Hard Voting**: 다수결 원칙. 가장 많은 모델이 선택한 클래스를 최종 예측으로 선정.
*   **Soft Voting**: 각 모델의 **예측 확률(Probability)**을 평균 내어, 가장 높은 확률을 가진 클래스를 선정. (일반적으로 성능이 더 좋음)

```python
from sklearn.ensemble import VotingClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.neighbors import KNeighborsClassifier

# 개별 모델 생성
lr_clf = LogisticRegression()
knn_clf = KNeighborsClassifier()

# VotingClassifier 생성 (soft voting)
vo_clf = VotingClassifier(estimators=[('LR', lr_clf), ('KNN', knn_clf)], voting='soft')
vo_clf.fit(X_train, y_train)
```

---

## 3. 배깅 (Bagging) - Random Forest
**랜덤 포레스트(Random Forest)**는 배깅의 대표주자입니다. '나무(Decision Tree)'가 모여서 '숲(Forest)'을 이룹니다.

*   **특징**:
    *   각 트리는 전체 데이터에서 중복을 허용해 샘플링(Bootstrap)한 데이터로 학습합니다.
    *   노드 분할 시 전체 특성이 아니라 무작위로 선택된 일부 특성만 고려합니다. (다양성 확보)
    *   오버피팅에 강하고, 범용적인 성능이 뛰어납니다.
*   **주요 파라미터 (`sklearn.ensemble.RandomForestClassifier`)**:
    *   `n_estimators`: 트리의 개수. (많을수록 좋지만 시간 소요)
    *   `max_depth`, `min_samples_split`: 개별 트리의 과적합 방지 파라미터.

```python
from sklearn.ensemble import RandomForestClassifier

rf_clf = RandomForestClassifier(n_estimators=100, random_state=0)
rf_clf.fit(X_train, y_train)
```

---

## 4. 부스팅 (Boosting)
틀린 문제에 가중치를 두어 더 집중적으로 학습하는 방식입니다. 오답 노트 학습법과 유사합니다.

### 4.1 GBM (Gradient Boosting Machine)
*   **경사 하강법(Gradient Descent)**을 이용해 잔차(Residual, 오차)를 줄여나갑니다.
*   성능은 좋지만 학습 시간이 매우 오래 걸립니다.

### 4.2 XGBoost (eXtreme Gradient Boosting)
GBM의 느린 속도를 개선하고 과적합 규제 기능을 강화한 라이브러리입니다.
*   **특징**: 병렬 학습 지원, Tree Pruning(가지치기), Early Stopping(조기 종료) 기능.
*   **주요 파라미터**:
    *   `n_estimators`: 부스팅 반복 횟수.
    *   `learning_rate`: 학습률. (작으면 촘촘하게 학습하지만 오래 걸림)
    *   `max_depth`: 트리의 깊이.

```python
from xgboost import XGBClassifier

xgb_wrapper = XGBClassifier(n_estimators=400, learning_rate=0.1, max_depth=3)
xgb_wrapper.fit(X_train, y_train)
```

### 4.3 LightGBM
XGBoost보다 더 가볍고 빠르면서도 비슷한 성능을 냅니다.
*   **특징**: 리프 중심 트리 분할(Leaf-wise) 방식을 사용하여 비대칭적인 트리를 생성, 손실을 더 크게 줄입니다.
*   대용량 데이터 처리에 적합하지만, 데이터가 너무 적으면 과적합 가능성이 있습니다.

```python
from lightgbm import LGBMClassifier

lgbm_wrapper = LGBMClassifier(n_estimators=400)
lgbm_wrapper.fit(X_train, y_train)
```

---

## 5. 인사이트 (Insights)

### 🌟 Key Takeaways
1.  **랜덤 포레스트는 기본**: 어떤 문제든 준수한 성능을 보장하는 베이스라인 모델로 훌륭합니다.
2.  **부스팅은 성능 최적화**: Kaggle 등 경진대회 상위권은 대부분 XGBoost, LightGBM, CatBoost 등의 부스팅 계열이 차지합니다.
3.  **조기 종료(Early Stopping)**: 부스팅 계열 사용 시, 더 이상 성능이 오르지 않으면 학습을 멈추는 기능을 적극 활용하여 시간을 절약하세요.
