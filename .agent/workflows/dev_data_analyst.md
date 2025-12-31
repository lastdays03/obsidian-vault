---
description: 데이터 분석의 전 과정(질의 정의 -> EDA -> 심층 분석 -> 리포팅)을 체계적으로 가이드하는 워크플로우입니다.
---

# Data Analyst Workflow

Python 생태계(Jupyter, Pandas, Seaborn)를 활용하여 데이터에서 인사이트를 도출하는 전문 분석 워크플로우입니다. OSEMN 방법론을 따릅니다.

### 1단계: 분석 환경 및 목표 정의 (Environment & Goal)
1.  **Notebook Setup**: `docs/analysis/` 또는 `notebooks/` 경로에 새로운 Jupyter Notebook(`01_EDA_[주제].ipynb`)을 생성합니다.
2.  **Objective**: 첫 번째 셀에 마크다운으로 해결하고자 하는 질문(Question)을 정의합니다.
3.  **Library Loading**: 필수 라이브러리(`pandas`, `matplotlib`, `seaborn`)를 임포트하고 한글 폰트 설정을 완료합니다.

### 2단계: 데이터 적재 및 전처리 (Obtain & Scrub)
1.  **Data Loading**: `pd.read_csv()` 등으로 데이터를 로드하고 `df.head()`로 확인합니다.
2.  **Sanity Check**: `df.info()`, `df.describe()`로 결측치(Null)와 이상치를 식별합니다.
3.  **Cleaning**: 결측치 처리(삭제/대체) 및 데이터 타입 변환 코드를 작성합니다.

### 3단계: 탐색적 데이터 분석 (Explore - EDA)
1.  **Univariate Analysis**: 개별 변수의 분포를 히스토그램(`sns.histplot`)으로 확인합니다.
2.  **Bivariate Analysis**: 변수 간의 관계를 산점도(`sns.scatterplot`)나 상관계수(`df.corr()`, Heatmap)로 파악합니다.
3.  **Insight Logging**: 발견된 특징이나 특이점을 마크다운 셀에 즉시 기록합니다.

### 4단계: 심층 분석 및 모델링 (Model & Interpret)
1.  **Deep Dive**: 가설 검증을 위한 통계적 분석(t-test 등)이나 패턴 마이닝을 수행합니다.
2.  **Visualization**: 주장을 뒷받침할 결정적인 시각화 차트를 생성합니다.
3.  **Reporting**: 최종 결론(Key Findings)과 액션 아이템을 노트북 하단에 요약하거나 별도 보고서로 내보냅니다.
