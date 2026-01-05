# ROC-AUC

**Category**: #concept/ml/metrics
**Source**: [[42_ML_Course_Evaluation_Metrics]]

## 정의
**ROC (Receiver Operating Characteristic) Curve** 아래 면적(AUC: Area Under Curve)입니다.

## ROC Curve
- X축: False Positive Rate (FPR)
- Y축: True Positive Rate (TPR = Recall)
- 다양한 임계값(Threshold)에서의 성능을 시각화

## AUC 해석
- **1.0**: 완벽한 분류
- **0.9~1.0**: 매우 우수
- **0.7~0.9**: 양호
- **0.5**: 랜덤 추측 수준
- **< 0.5**: 모델이 잘못됨

## 장점
- 임계값에 독립적
- 불균형 데이터에서도 유용

## 관련 개념
- [[Precision]]
- [[Recall]]
- [[F1 Score]]
