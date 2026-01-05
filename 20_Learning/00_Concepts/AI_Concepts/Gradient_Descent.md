# Gradient Descent (경사 하강법)

**Category**: #concept/ml/optimization
**Source**: [[31_ML_Course_Gradient_Descent]]

---

## 정의 (Definition)
손실 함수(Loss Function)를 **최소화**하기 위해 기울기(Gradient)의 반대 방향으로 파라미터를 조금씩 업데이트하는 최적화 알고리즘입니다.

## 비유 (Analogy)
안개 낀 산에서 가장 낮은 곳으로 내려가려고 할 때, 현재 위치에서 가장 가파른 내리막 방향으로 한 걸음씩 이동하는 것과 같습니다.

## 수식 (Formula)
$$\theta = \theta - \alpha \cdot \nabla J(\theta)$$

- $\theta$: 모델 파라미터 (가중치, 편향)
- $\alpha$: 학습률 (Learning Rate)
- $\nabla J(\theta)$: 손실 함수의 기울기 (Gradient)

## 학습률 (Learning Rate)의 중요성
- **너무 크면**: 최적점을 지나쳐 발산 (Divergence)
- **너무 작으면**: 학습 속도가 매우 느림, 지역 최소값(Local Minimum)에 갇힐 위험

## 변형 알고리즘 (Variants)
- **Batch Gradient Descent**: 전체 데이터로 기울기 계산 (느리지만 안정적)
- **Stochastic Gradient Descent (SGD)**: 샘플 하나씩 업데이트 (빠르지만 불안정)
- **Mini-Batch Gradient Descent**: 일부 샘플(배치)로 업데이트 (절충안, 가장 많이 사용)
- **Adam, RMSprop**: 학습률을 자동 조정하는 고급 최적화 기법

## 관련 개념 (Related Concepts)
- [[Hyperparameter Tuning]]
- [[Regularization]]
