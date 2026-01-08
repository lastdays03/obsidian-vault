---
tags: [knowledge/topic, methodology/antigravity]
Up: [[Antigravity_MOC]]
---

# Antigravity Skills (Development & Workflow Guide)

이 문서는 **Google Antigravity** 환경에서 AI 에이전트와 함께 효율적으로 협업하기 위한 **Workflows(Skill)** 및 **설정 표준**을 정의하는 마스터 가이드입니다. [[03_Claude_Skills_Methodology|Claude Skills 방법론]]을 Antigravity에 맞게 최적화하였습니다.

## 🚀 시작하기 (Getting Started)

새로운 프로젝트나 기존 프로젝트에 이 표준 설정을 적용하려면 `init_agent.sh` 스크립트를 사용합니다. (상세: [[10_Utilities_and_Tools]])

### 설치 및 동기화

`.agent/scripts/init_agent.sh` 스크립트는 공통 설정(`.agent/`)을 타겟 프로젝트 root에 **심볼릭 링크**로 연결하고, `.gitignore`에 자동으로 추가합니다.

```bash
# 타겟 프로젝트 루트에서 실행 (예시)
cd ~/dev/workspace/my_new_project

# 스크립트 실행 (상대 경로 예시)
../claude_skills/.agent/scripts/init_agent.sh
```

---

## 🛠️ 워크플로우 라이브러리 (Workflow Library)

에이전트에게 다음과 같은 슬래시 커맨드(`/`)를 입력하여 전문화된 작업을 시작하세요. 상세한 구조는 [[01_Workflow_Structure]]를 참고하세요.

### 1. 전략 및 시작 (Strategy & Start)
프로젝트 초기 설정 및 방향성 수립 단계입니다. (참고: [[02_Project_Kickoff_Analysis]])

| 커맨드                    | 설명                                                                                  |
| :------------------------ | :------------------------------------------------------------------------------------ |
| **/obsi_project_kickoff** | **[Obsidian]** 표준 폴더 구조, Overview, Task 생성으로 프로젝트를 빠르게 시작합니다.  |
| **/dev_init**             | **[Dev]** 개발 전용 환경(VSCode)에서 `src`, `docs` 구조와 `README.md`를 초기화합니다. |

### 2. 개발 및 구현 (Development & Engineering)
견고한 코드 작성과 제품 구현을 위한 워크플로우입니다. (참고: [[04_Feature_Planner_Deep_Dive]], [[06_Lifecycle_Combo]])

| 커맨드                    | 설명                                                                              |
| :------------------------ | :-------------------------------------------------------------------------------- |
| **/dev_feature_planner**  | **[Dev]** TDD, 리스크 평가, 롤백 전략을 포함한 엔지니어링 표준 기능을 구현합니다. |
| **/dev_notebook_refiner** | **[Dev]** Jupyter Notebook의 코드 품질, 문서화, 실행 안정성을 개선합니다.         |
| **/dev_export**           | **[Dev]** 개발 문서를 Obsidian Inbox로 단순 백업(Export)합니다.                   |
| **/obsi_archive_project** | **[Obsidian]** 완료된 프로젝트를 정리하고 검증하여 연도별 아카이브로 이동합니다.  |

### 3. 학습 및 연구 (Learning & Research)
새로운 기술 습득과 심층 이해를 돕습니다. (참고: [[09_Study_Planner_Workflow]])

| 커맨드                 | 설명                                                                                 |
| :--------------------- | :----------------------------------------------------------------------------------- |
| 커맨드                 | 설명                                                                                 |
| :--------------------- | :----------------------------------------------------------------------------------- |
| **/dev_study_planner** | **[Deep]** 파인만 기법과 Break & Fix를 활용하여 개념을 완벽하게 체득(Mastery)합니다. |
| **/dev_trend_tracker** | **[Quick]** 최신 기술 트렌드나 도구를 빠르게 파악하고 3줄로 요약하여 아카이빙합니다. |

### 4. 데이터 분석 (Data Analysis)
데이터로부터 인사이트를 도출합니다. (참고: [[11_Data_Analyst_Workflow]])

| 커맨드                | 설명                                                                           |
| :-------------------- | :----------------------------------------------------------------------------- |
| **/dev_data_analyst** | **[Data]** 질의 정의 → EDA → 심층 분석 → 리포팅의 전체 분석 과정을 수행합니다. |

### 5. 지식 관리 (Knowledge Management)
파편화된 정보를 체계적인 지식 자산으로 변환합니다. (참고: [[05_Knowledge_Workflows]], [[07_Knowledge_Combo]])

| 커맨드                        | 설명                                                                         |
| :---------------------------- | :--------------------------------------------------------------------------- |
| **/obsi_concept_distiller**   | 텍스트에서 핵심 개념을 추출하여 지식 베이스와 연결합니다.                    |
| **/obsi_knowledge_harvester** | 프로젝트의 실전 노트(Topic Note)를 영구 지식 노트(20_Learning)로 이관합니다. |
| **/obsi_knowledge_refiner**   | 기존 노트의 내용을 심화하고 시각화하여 "골드 스탠다드" 지식으로 만듭니다.    |
| **/obsi_moc_builder**         | 노트들의 연관 관계를 분석하여 구조화된 목차(MOC)를 생성합니다.               |
| **/obsi_weekly_review**       | 주간 회고를 수행하고 액션 아이템을 도출하여 GTD 시스템을 유지합니다.         |

---

## 🧩 개념 및 매핑 (Concepts)

이 가이드는 Claude Code의 개념을 Antigravity의 네이티브 기능으로 매핑하여 구현되었습니다.

| 개념          | Antigravity 구현                         | 역할                          |
| :------------ | :--------------------------------------- | :---------------------------- |
| **Skill**     | **Workflow** (`.agent/workflows/*.md`)   | 에이전트의 수행 절차 정의     |
| **Plan**      | **Artifacts** (`implementation_plan.md`) | 작업 계획 및 설계 문서화      |
| **Checklist** | **Task Boundary** (`task.md`)            | 실시간 진행 상황 및 상태 추적 |
| **Gates**     | **Notification** (`notify_user`)         | 사용자 승인 및 피드백 루프    |

### 커스텀 워크플로우 만들기
나만의 스킬을 추가하려면 `.agent/workflows/` 경로에 마크다운 파일을 생성하세요.

1. **파일 생성**: `my_custom_skill.md`
2. **헤더 작성**:
   ```markdown
   ---
   description: 이 스킬이 수행하는 작업에 대한 설명
   ---
   ```
3. **지침 작성**: 에이전트가 수행해야 할 단계를 명확한 자연어로 서술합니다.

3. **지침 작성**: 에이전트가 수행해야 할 단계를 명확한 자연어로 서술합니다.

---

## 🏗️ 아키텍처: 참조 분리 (Reference Separation Pattern)

모든 워크플로우는 **"가벼운 실행 로직(Workflow)"**과 **"무거운 상세 표준(Reference)"**으로 분리되어 설계되었습니다.

```text
.agent/
├── workflows/             # [How-to] 에이전트의 행동 순서 (Lightweight)
│   └── dev_data_analyst.md
└── references/            # [Standard] 품질 기준 및 템플릿 (Heavy)
    └── dev_data_analyst/
        ├── SKILL.md       # (O) 품질 표준, 철학, 체크리스트
        └── template.md    # (O) 사용자가 편집하기 쉬운 마크다운 양식
```

### 이 패턴의 장점
1.  **효율성**: 에이전트는 작업을 시작할 때만 필요한 Reference를 읽어 토큰을 절약합니다.
2.  **유지보수**: 템플릿이나 기준을 수정할 때, 복잡한 워크플로우 로직을 건드릴 필요가 없습니다.
3.  **표준화**: `SKILL.md`는 해당 도메인의 "Single Source of Truth" 역할을 합니다.

### 커스텀 워크플로우 만들기
나만의 스킬을 추가하려면 위 구조를 따르는 것을 권장합니다.

1.  **Reference 생성**: `.agent/references/{skill_name}/SKILL.md`에 규칙 작성.
2.  **Workflow 생성**: `.agent/workflows/{skill_name}.md`에서 `SKILL.md`를 로드하도록 지시.

---

## 🔐 보안 가이드 (Security Guide)

Antigravity 에이전트의 권한을 관리하기 위한 보안 설정은 `.agent/SECURITY.md`에 정의되어 있습니다.

*   **Reference**: `.agent/SECURITY`
*   **내용**: 3단계 보안 프로필(Strict, Balanced, Efficiency) 및 터미널 명령어 Allow/Deny List.
*   **사용법**: 해당 문서를 참고하여 Antigravity 설정 화면(Settings > File Access / Terminal)에 값을 적용하세요.
*   **팁**: 설정 입력이 번거로운 경우, **Prefix 매칭**을 활용한 간편 설정 가이드도 포함되어 있습니다.

## 📂 프로젝트 구조

```text
claude_skills/
├── README.md               # 프로젝트 가이드
└── .agent/                 # 에이전트 설정 원본
    ├── scripts/
    │   └── init_agent.sh   # 설정 동기화 스크립트
    ├── rules.md            # 기본 규칙
    ├── SECURITY.md         # 보안 설정 가이드
    ├── references/         # 참조 문서 (dev_feature_planner resources 등)
    └── workflows/          # 워크플로우 정의 파일들 (*.md)
```

### 글로벌 동기화 (`scripts/sync_to_global.sh`)

현재 프로젝트의 워크플로우와 규칙을 **글로벌 환경**(`~/.gemini/...`)으로 내보냅니다.

```bash
# 실행
.agent/scripts/sync_to_global.sh
```

**동기화 경로:**
*   워크플로우: `~/.gemini/antigravity/global_workflows`
*   규칙: `~/.gemini/GEMINI.md`

### 자동 동기화 (`scripts/watch_and_sync.sh`)

워크플로우 파일 변경 시 자동으로 `sync_to_global.sh`를 실행합니다.
**실행 즉시 동기화(Initial Sync)**를 수행하므로, 켜두기만 하면 항상 최신 상태가 보장됩니다.

```bash
# 실행
.agent/scripts/watch_and_sync.sh
```

## 💡 Key Insights
*   **표준의 전파**: `claude_skills`는 단순한 저장소가 아니라, 모든 연동된 프로젝트에 '표준'을 전파하는 컨트롤 타워입니다.
*   **스킬의 확장**: 워크플로우 파일만 추가하면 에이전트의 능력(Skill)을 무한히 확장할 수 있습니다.
