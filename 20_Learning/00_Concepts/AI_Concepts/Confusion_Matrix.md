# Confusion Matrix (오차 행렬)

**Category**: #concept/ml/metrics
**Source**: [[42_ML_Course_Evaluation_Metrics]]

## 정의
분류 모델의 예측 결과를 **4가지 경우**로 나누어 표현한 행렬입니다.

## 구성 요소

|                    | 예측: Positive      | 예측: Negative      |
| ------------------ | ------------------- | ------------------- |
| **실제: Positive** | TP (True Positive)  | FN (False Negative) |
| **실제: Negative** | FP (False Positive) | TN (True Negative)  |

- **TP**: 양성을 양성으로 올바르게 예측
- **TN**: 음성을 음성으로 올바르게 예측
- **FP**: 음성을 양성으로 잘못 예측 (Type I Error)
- **FN**: 양성을 음성으로 잘못 예측 (Type II Error)

## 활용
모든 분류 지표의 기초가 됩니다:
- Accuracy = (TP + TN) / Total
- Precision = TP / (TP + FP)
- Recall = TP / (TP + FN)

## 관련 개념
- [[Precision]]
- [[Recall]]
- [[F1 Score]]
