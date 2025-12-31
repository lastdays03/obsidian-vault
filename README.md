# Obsidian Developer Vault

이 볼트는 **Developer Portfolio**와 **Personal Knowledge Management (PKM)**를 통합한 전문가용 옵시디언 볼트입니다. **Antigravity AI Agent**와 연동되어 자동화된 워크플로우를 제공합니다.

---

## 📂 Directory Structure (PARA + Knowledge)

```text
/
├── 00_Inbox/      # 📥 수집: 임시 메모, 스크랩 (주 1회 비우기)
├── 10_Projects/   # 🚀 실행: 진행 중인 프로젝트 (Portfolio Source)
├── 20_Learning/   # 🧠 지식: 기술, 개념, 도메인 지식 (영구 보존)
├── 30_Journal/    # 📅 기록: Daily Note, 주간 회고, 삶의 로그
├── 40_Resources/  # 📚 서재: 책, 아티클, 영상, 리소스 아카이브
├── 90_Archives/   # 🗄️ 보관: 완료된 프로젝트, 지난 연도 저널
└── 99_System/     # ⚙️ 설정: 템플릿, 첨부파일, 시스템 설정
```

---

## 🤖 Antigravity Workflows

AI 에이전트(`<Cmd+L>`)에게 아래 명령어를 입력하여 워크플로우를 실행하세요.

### 🏗️ Project & Cycle
| Command                 | Description                                                                 |
| :---------------------- | :-------------------------------------------------------------------------- |
| `/obsi_project_kickoff` | **프로젝트 시작**: 폴더 구조, README, Plan 자동 생성 (Dev/Study 모드 지원). |
| `/obsi_archive_project` | **프로젝트 종료**: 완료된 프로젝트를 정리하여 `90_Archives`로 이동.         |
| `/obsi_weekly_review`   | **주간 회고**: `00_Inbox` 청소, 할 일 점검, 한 주의 배움 요약.              |

### 🧠 Knowledge Management
| Command                     | Description                                                                     |
| :-------------------------- | :------------------------------------------------------------------------------ |
| `/obsi_knowledge_harvester` | **수확 (Harvest)**: 프로젝트/인박스의 노트를 `20_Learning`으로 이관.            |
| `/obsi_concept_distiller`   | **추출 (Distill)**: 텍스트에서 핵심 개념을 뽑아 정의하고 연결.                  |
| `/obsi_knowledge_refiner`   | **경작 (Refine)**: 기존 노트의 품질을 '골드 스탠다드'로 고도화 (인사이트 도출). |
| `/obsi_moc_builder`         | **지도 (MOC)**: 흩어진 노트들을 묶어 목차(Map of Content) 생성.                 |

### 💻 Development Support
| Command                | Description                                                             |
| :--------------------- | :---------------------------------------------------------------------- |
| `/dev_init`            | **환경 초기화**: `.agent` 설정 및 폴더 구조 세팅.                       |
| `/dev_feature_planner` | **기능 구현**: TDD 기반의 구현 계획 수립 (Plan -> Implement -> Verify). |
| `/dev_study_planner`   | **기술 학습**: "고장내며 배우기" 방식의 실습형 학습 플래너.             |
| `/dev_data_analyst`    | **데이터 분석**: OSEMN 방법론 기반의 EDA 및 리포팅 가이드.              |
| `/dev_export`          | **산출물 내보내기**: 개발 산출물을 옵시디언으로 복사.                   |

---

## 🔄 Sync Strategy (iCloud + Git)

이 볼트는 두 가지 동기화 방식을 병행하여 안정성과 편리함을 동시에 추구합니다.

### 1. iCloud Drive
*   **역할**: 기기 간 **실시간 동기화** (Mac ↔ iPhone/iPad).
*   **대상**: 모든 파일 (설정 포함).

### 2. Git (GitHub)
*   **역할**: **버전 관리** 및 안전한 백업.
*   **Remote**: [lastdays03/obsidian-vault](https://github.com/lastdays03/obsidian-vault)
*   **Ignore 정책**: `.obsidian/` (설정 충돌 방지), `.smart-env` (보안) 제외.
*   **사용법**:
    ```bash
    git add .
    git commit -m "Backup notes"
    git push
    ```

---

## 🚀 Quick Start
1. **아침**: `30_Journal`에서 오늘 날짜의 Daily Note를 엽니다 (`Cmd+Shift+D` w/ Calendar).
2. **생각나면**: `00_Inbox`에 빠른 메모를 남깁니다.
3. **작업 시**: `10_Projects`의 해당 프로젝트 노트에서 `/feature_planner` 등을 활용합니다.
4. **일요일**: `/weekly_review`를 실행하여 한 주를 정리합니다.
