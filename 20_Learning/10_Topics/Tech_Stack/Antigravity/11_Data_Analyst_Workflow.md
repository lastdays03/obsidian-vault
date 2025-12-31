# 11. Data Analyst Workflow

**Goal**: 데이터에서 인사이트를 추출하는 "탐색적 데이터 분석(EDA)" 과정을 체계화한 **/dev_data_analyst** 워크플로우를 이해합니다.
**Reference**: `.agent/workflows/data_analyst.md`

## 1. 개요 (Overview)
단순한 코딩 작업과 달리, 데이터 분석은 **"질문 -> 탐색 -> 발견 -> 보고"**의 반복적인 사이클을 가집니다. 이 워크플로우는 **[[OSEMN]]** 방법론(Obtain, Scrub, Explore, Model, iNterpret)을 기반으로 Python 데이터 분석 환경 가이드를 제공합니다.

## 2. 분석 파이프라인 (The Pipeline)

### Phase 1: Environment & Goal (환경 및 목표)
- **Goal Definition**: 해결하고자 하는 질문을 노트북 최상단에 명시합니다. (목적 없는 분석 방지)
- **Setup**: `pandas`, `seaborn` 등 필수 라이브러리와 한글 폰트 설정을 완료합니다.

### Phase 2: Obtain & Scrub (적재 및 전처리)
- **Sanity Check**: `info()`, `describe()`로 데이터의 기초 체력을 진단합니다.
- **Cleaning**: 결측치(Null)와 이상치(Outlier)를 처리하여 분석 가능한 상태로 만듭니다.

### Phase 3: Explore (EDA - 탐색적 분석)
- **Univariate**: 히스토그램 등으로 개별 변수의 분포를 봅니다.
- **Bivariate**: 산점도, 상관계수 히트맵으로 변수 간 관계를 파악합니다.
- **Insight Logging**: 차트를 보고 발견한 점을 즉시 마크다운으로 기록합니다.

### Phase 4: Model & Interpret (심층 분석 및 보고)
- **Deep Dive**: 가설을 검증하고, 결정적인 시각화(Chart)를 생성합니다.
- **Reporting**: 결론(Key Findings)과 액션 아이템을 도출합니다.

## 3. 실습 (Action Item)
1. `/dev_data_analyst` 워크플로우를 실행합니다.
2. 분석하고 싶은 CSV 데이터(또는 샘플 데이터)를 준비합니다.
3. 워크플로우를 따라 EDA를 수행하고, 흥미로운 인사이트 1가지를 찾아내세요.
