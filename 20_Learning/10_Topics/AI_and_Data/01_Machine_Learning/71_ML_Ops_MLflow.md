---
Source: study_machine_learning
---

# MLOps 기초: MLflow 실험 추적
# 태그: #MLOps #MLflow #ExperimentTracking #ModelManagement

이 문서는 `C2_M4` 주차의 노트북(`Exp_MLflow`)에서 다루어진 머신러닝 실험 관리 도구인 **MLflow**의 핵심 기능을 요약 정리한 것입니다.

---

## 1. 개요 및 필요성
머신러닝 모델을 개발할 때 파라미터(Learning Rate, Batch Size 등)를 매번 바꾸며 수십 번 실험하게 됩니다.
*   **문제점**: "아까 성능 제일 좋았던 설정이 뭐였지?" (기억에 의존, 엑셀 정리의 한계)
*   **해결**: **MLflow**를 사용하면 모든 실험의 파라미터, 메트릭(정확도 등), 모델 파일을 자동으로 기록하고 비교할 수 있습니다.

---

## 2. 핵심 기능 실습

### 2-1. 실험(Experiment) 설정
```python
import mlflow

# 실험 이름 설정 (없으면 자동 생성)
mlflow.set_experiment("My_First_MLflow_Exp")
```

### 2-2. 실험 실행 및 로깅 (Logging)
`mlflow.start_run()` 블록 안에서 학습 코드를 실행하면 됩니다.

*   `log_param(key, value)`: 하이퍼파라미터 기록 (예: n_estimators, max_depth)
*   `log_metric(key, value)`: 성능 지표 기록 (예: accuracy, loss)
*   `sklearn.log_model(model, name)`: 학습된 모델 객체 저장

```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

with mlflow.start_run():
    # 1. 파라미터 설정 및 기록
    n_est = 100
    depth = 5
    mlflow.log_param("n_estimators", n_est)
    mlflow.log_param("max_depth", depth)

    # 2. 모델 학습
    clf = RandomForestClassifier(n_estimators=n_est, max_depth=depth)
    clf.fit(X_train, y_train)

    # 3. 성능 평가 및 기록
    pred = clf.predict(X_test)
    acc = accuracy_score(y_test, pred)
    mlflow.log_metric("accuracy", acc)
    
    # 4. 모델 저장
    mlflow.sklearn.log_model(clf, "random_forest_model")
    
    print(f"Logged: accuracy={acc}")
```

### 2-3. UI 확인
터미널에서 `mlflow ui` 명령어를 실행하면 웹 브라우저(http://localhost:5000)에서 실험 결과들을 표와 차트로 비교 분석할 수 있습니다.

---

## 3. 인사이트 (Insights)
*   **실험의 자산화**: 휘발되기 쉬운 실험 결과들을 영구적인 자산으로 남길 수 있습니다.
*   **재현성 확보**: 언제든 과거의 최고 성능 모델을 똑같은 환경과 파라미터로 다시 만들어낼 수 있습니다. (Reproducibility)
*   **협업의 필수품**: 팀원들과 실험 결과를 공유하고, 배포 승인 프로세스를 자동화하는 MLOps 파이프라인의 기초가 됩니다.
