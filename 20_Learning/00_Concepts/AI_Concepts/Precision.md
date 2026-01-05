# Precision (정밀도)

**Category**: #concept/ml/metrics
**Source**: [[42_ML_Course_Evaluation_Metrics]]

## 정의
모델이 **Positive로 예측한 것 중** 실제로 Positive인 비율입니다.

$$\text{Precision} = \frac{TP}{TP + FP}$$

## 의미
"양성이라고 예측한 것이 얼마나 정확한가?"

## 중요한 경우
- **False Positive 비용이 큰 경우**
- 예: 스팸 메일 분류 (정상 메일을 스팸으로 잘못 분류하면 안 됨)

## 관련 개념
- [[Recall]]
- [[F1 Score]]
- [[Confusion Matrix]]
