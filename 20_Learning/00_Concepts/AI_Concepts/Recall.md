# Recall (재현율)

**Category**: #concept/ml/metrics
**Source**: [[42_ML_Course_Evaluation_Metrics]]

## 정의
**실제 Positive 중** 모델이 Positive로 예측한 비율입니다.

$$\text{Recall} = \frac{TP}{TP + FN}$$

## 의미
"실제 양성을 얼마나 잘 찾아내는가?"

## 중요한 경우
- **False Negative 비용이 큰 경우**
- 예: 암 진단 (암 환자를 정상으로 잘못 판단하면 안 됨), 사기 탐지

## Precision과의 Tradeoff
- Recall을 높이면 Precision이 낮아짐
- 둘의 균형을 맞추는 것이 중요

## 관련 개념
- [[Precision]]
- [[F1 Score]]
- [[Confusion Matrix]]
