# Antigravity Skills 적용 가이드

이 가이드는 Claude Code의 "Skills" 개념(`SKILL.md` 참조)을 Google Antigravity 환경에 맞게 조정하여 사용하는 방법을 설명합니다.

## 개념 매핑 (Conceptual Mapping)

사용자가 제공한 문서에 정의된 "Skill"(복잡한 코딩 작업을 완료하기 위한 구조화된 프로세스)은 **Antigravity Workflows** 및 **Agentic Mode Artifacts 시스템**과 직접적으로 매핑됩니다.

| Claude Code 개념                | Antigravity 대응 기능                    | 설명                                                                             |
| ------------------------------- | ---------------------------------------- | -------------------------------------------------------------------------------- |
| **Skill 정의**                  | **Workflow** (`.agent/workflows/*.md`)   | 에이전트가 따라야 할 단계별 절차를 정의합니다.                                   |
| **Plan 템플릿**                 | **Artifacts** (`implementation_plan.md`) | Antigravity는 계획 수립을 위한 전용 아티팩트를 이미 내장하고 있습니다.           |
| **단계(Phases) & 체크리스트**   | **Task Boundaries** (`task.md`)          | `task.md` 아티팩트는 세부 진행 상황을 추적하는 살아있는 문서 역할을 합니다.      |
| **사용자 승인**                 | **`notify_user` 도구**                   | `BlockedOnUser: true`를 사용하여 사용자 검토를 위해 작업을 일시 중지합니다.      |
| **품질 게이트 (Quality Gates)** | **Verification 모드**                    | `implementation_plan.md`와 `walkthrough.md`에 명시적인 검증 단계를 문서화합니다. |

## Antigravity에 Skill 이식하는 방법

**Feature Planner**와 같은 Skill을 Antigravity에 "설치"하려면 **Workflow**를 생성해야 합니다.

### 1. Workflow 파일 생성
작업 공간의 `.agent/workflows/dev_feature_planner.md` 경로에 마크다운 파일을 생성합니다. (이미 생성됨)

### 2. 단계 정의 (Define the Steps)
`SKILL.md`의 단계를 에이전트를 위한 자연어 지침으로 변환합니다. 구체적인 도구(Tool) 사용법을 명시하세요.

**변환 예시:**

*   **요구사항 분석** → 지침: "관련 핵심 파일을 읽고 `task.md`를 단계별로 분류하여 업데이트하세요."
*   **계획 수립** → 지침: "TDD 구조에 따라 `implementation_plan.md`를 생성하세요."
*   **품질 게이트** → 지침: "VERIFICATION 모드에서 작업을 완료하기 전에 모든 테스트가 통과하는지 확인하세요."

### 3. Skill 사용
파일이 생성되면 다음과 같이 요청하여 Skill을 실행할 수 있습니다: *"기능 A에 대해 feature planner 워크플로우를 실행해 줘"*

## 워크플로우 효율적 활용 가이드 (Workbook Efficiency Guide)

현재 구축된 워크플로우들을 목적에 맞게 선택하여 사용하는 것이 생산성의 핵심입니다. 다음 분류를 참고하여 상황에 맞는 도구를 선택하세요.

### 1. 전략 및 시작 (Strategy & Start)
프로젝트나 아이디어를 처음 구체화할 때 사용합니다.

*   **/obsi_project_kickoff**
    *   **When**: 새로운 프로젝트 폴더를 생성할 때.
    *   **Why**: 폴더 구조, `Overview.md`, 기본 `task.md`를 표준에 맞춰 자동 생성하여 세팅 시간을 단축합니다.
    *   **Effect**: "어디서부터 시작하지?"라는 고민 제거.

*   **/dev_init**
    *   **When**: 옵시디언이 아닌 **개발 전용(VSCode)** 환경에서 프로젝트를 초기화할 때.
    *   **Why**: 표준 디렉토리 구조(`src`, `docs`)와 `README.md` 메타데이터를 빠르게 생성합니다.

### 2. 개발 및 구현 (Development & Engineering)
실제 코드를 작성하고 프로덕트를 만들 때 사용합니다.

*   **/dev_feature_planner**
    *   **When**: 사용자에게 배포할 **기능(Feature)**을 개발할 때.
    *   **Why**: TDD, 리스크 평가, 롤백 전략 등 '엔지니어링 표준'을 준수해야 할 때 필수적입니다.
    *   **Effect**: 버그 감소, 코드 품질 향상, 명확한 진척도 관리.
    *   **Vs Study Planner**: 코드를 고장내보며 실험하는 것이 아니라, **안전하고 견고하게** 만드는 것이 목표입니다.

*   **/obsi_archive_project**
    *   **When**: 프로젝트가 완전히 종료되었을 때.
    *   **Why**: 의존성 검사 후 안전하게 아카이빙하여 작업 공간을 정리합니다.

*   **/dev_export**
    *   **When**: 개발/학습 중 작성한 문서를 Obsidian 지식 베이스로 **단순 백업**하고 싶을 때.
    *   **Why**: 복잡한 연동 없이 결과물을 안전하게 `00_Inbox`로 보냅니다.

### 3. 학습 및 연구 (Learning & Research)
새로운 기술을 익히거나 개념을 정립할 때 사용합니다.

*   **/dev_study_planner**
    *   **When**: 새로운 언어, 프레임워크, 라이브러리를 **공부**할 때.
    *   **Why**: 기능 구현과 달리, **"코드를 고장내보고(Break & Fix)"**, "이론을 요약"하는 학습 특화 단계가 포함되어 있습니다.
    *   **Effect**: 단순 복사/붙여넣기가 아닌 깊이 있는 원리 이해.

### 4. 데이터 분석 (Data Analysis)
데이터에서 가치를 추출할 때 사용합니다.

*   **/dev_data_analyst**
    *   **When**: 확보된 데이터셋에 대해 EDA(탐색적 분석) 및 리포팅이 필요할 때.
    *   **Why**: **Python/Jupyter** 환경에 최적화된 "분석-시각화-리포팅" 사이클을 따릅니다.
    *   **Effect**: 코드 생산이 목적이 아닌, **"인사이트 발견"**에 집중할 수 있습니다.

### 5. 지식 관리 (Knowledge Management)
흩어진 정보를 자산화할 때 사용합니다.

*   **/obsi_concept_distiller**
    *   **When**: 복잡한 텍스트나 대화 내용에서 **핵심 개념**만 뽑아내어 저장하고 싶을 때.
*   **/obsi_knowledge_harvester**
    *   **When**: 프로젝트 진행 중 작성한 메모를 영구적인 **지식 베이스(20_Learning)**로 옮길 때.
*   **/obsi_moc_builder**
    *   **When**: 노트가 너무 많아져서 **목차(Map of Content)**가 필요할 때.
*   **/obsi_knowledge_refiner**
    *   **When**: 기존 노트의 품질을 높이고, 내용을 심화하거나 시각적 자료(다이어그램)를 추가하고 싶을 때.
    *   **Why**: 단순한 메모를 **"골드 스탠다드"** 지식 자산으로 탈바꿈시킵니다.
    *   **Effect**: 구조화, 내용 보강(심화), 연결성 강화, 멀티모달(시각화) 확장.
*   **/obsi_weekly_review**
    *   **When**: 한 주를 마무리하며 할 일과 메모를 정리할 때 (GTD).

## 예시: "Feature Planner" 워크플로우

`.agent/workflows/dev_feature_planner.md` 파일을 참조하세요.

```markdown
---
description: TDD 단계와 품질 게이트를 사용하여 기능을 계획하고 구현합니다.
---

# Feature Planner Workflow

이 워크플로우는 엄격한 TDD 주기와 단계별 계획을 사용하여 기능을 구현하도록 안내합니다.
... (이하 생략)
```

## 요약

매번 `plan-template.md`를 수동으로 파싱할 필요가 없습니다. 해당 템플릿을 사용하는 *프로세스*를 **Antigravity Workflow**로 인코딩하면 "Skill"을 자동화할 수 있습니다.

## 유틸리티 및 도구 (Utilities & Tools)

프로젝트 간에 **일관된 에이전트 설정**을 유지하기 위해 제공되는 도구입니다.

### 동기화 스크립트 (`scripts/init_agent.sh`)

새로운 프로젝트나 기존 프로젝트에서 `claude_skills`의 공통 설정(`.agent` 폴더)을 사용하고 싶을 때 실행합니다.

**기능:**

*   공통 설정(`.agent` 내 `rules.md`, `workflows/`, `references/`)을 현재 프로젝트로 **복사(Copy)**합니다.
*   기존에는 심볼릭 링크 방식이었으나, VSCode/Git 호환성을 위해 **물리적 복사** 방식으로 변경되었습니다.
*   `.agent` 폴더는 Git 저장소에 **포함(Commit)**하여 프로젝트별로 설정을 독립적으로 관리합니다.

**사용법:**
터미널에서 설정하려는 프로젝트의 루트 디렉토리로 이동한 후, 상대 경로로 스크립트를 실행합니다.

```bash
# 예시: 새로운 프로젝트 'my_new_project'에서 실행 시
cd ~/dev/workspace/my_new_project
../claude_skills/scripts/init_agent.sh
```
