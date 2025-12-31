# Obsidian Vault Structure Guide: Developer Portfolio Edition

이 가이드는 **Developer Portfolio** 모델을 기반으로 설계된 옵시디언 볼트의 구조와 사용법, 네이밍 규칙을 설명합니다.

---

## 📂 Directory Structure Overview

```text
/
├── 00_Inbox/      # 📥 수집 통 : 모든 스크랩, 빠른 메모, 정리되지 않은 생각
├── 10_Projects/   # 🚀 포트폴리오 : 진행 중/완료된 프로젝트의 실행 공간
├── 20_Learning/   # 🧠 지식 베이스 : 기술, 개념, 언어별 깊은 학습 노트 (Archive 포함)
├── 30_Journal/    # 📅 히스토리 : Daily Note, 회고, 삶의 기록
├── 40_Resources/  # 📚 서재 : 참고 자료, 아티클, 책, 웹 클리핑
├── 90_Archives/   # 🗄️ 창고 : 완료된 프로젝트, 지난 연도의 저널 (보존용)
└── 99_System/     # ⚙️ 시스템 : 템플릿, 첨부파일(이미지), 시스템 설정
```

---

## 📝 Detailed Usage & Naming Conventions

### `00_Inbox`
- **역할**: "일단 저장"하는 곳. 분류에 대한 고민 없이 빠르게 정보를 수집합니다.
- **규칙**: 주 1회(Weekly Review) 비우기를 목표로 합니다.
- **파일명**: 자유 (ex: `빠른 메모.md`, `아이디어 스케치.md`)

### `10_Projects` (Portfolio)
- **역할**: 명확한 목표와 마감기한이 있는 프로젝트. 포트폴리오의 핵심 소스입니다.
- **구조**: `10_Projects/{Project_Name}/` 폴더 생성.
- **필수 파일**: `{Project_Name}_Overview.md` (MOC 역할)
- **파일명 규칙**: `{Project}_{Topic}.md` (ex: `Obsidian_Setup_Guide.md`)
- **Tag**: `#status/active`, `#status/completed`, `#project`

### `20_Learning` (Knowledge Base)
- **역할**: 프로젝트와 무관하게 평생 관리할 개념, 기술 지식.
- **구조**: `20_Learning/{Category}/{Topic}.md`
  - ex: `20_Learning/Language/Python.md`
  - ex: `20_Learning/CS/Algorithm/QuickSort.md`
- **연결**: 반드시 **"이 지식이 쓰인 프로젝트"**나 **"참고한 리소스"**로 링크를 남깁니다.
- **Tag**: `#topic/python`, `#concept`, `#til`

### `30_Journal` (History)
- **역할**: 시간 순서대로 나열되는 나의 기록.
- **구조**:
  - `30_Journal/Daily/YYYY-MM-DD.md`
  - `30_Journal/Weekly/YYYY-Www.md`
- **내용**: 오늘 한 일, 배운 점(TIL), 프로젝트 진행 상황 로그.

### `40_Resources`
- **역할**: 외부에서 가져온 오리지널 소스.
- **구조**:
  - `40_Resources/Books/`
  - `40_Resources/Articles/` (웹 스크랩)
  - `40_Resources/Videos/`

### `90_Archives`
- **역할**: 완료된 프로젝트나 오래된 저널을 보관하여 메인 폴더(`10_Projects`)를 가볍게 유지합니다.
- **구조**:
  - `90_Archives/Projects/2024/` : 연도별로 완료된 프로젝트 이동.
  - `90_Archives/Journal/2023/` : 지난 해의 일기장 보관.
- **규칙**: 프로젝트가 완료되면 `#status/done` 태그를 붙이고, 연말이나 분기 말에 이곳으로 이동시킵니다.

### `99_System`
- **역할**: 볼트 관리를 위한 메타 데이터.
- **구조**:
  - `99_System/Templates/` : 템플릿 파일들.
  - `99_System/Attachments/` : 이미지, PDF 등 첨부파일 (설정에서 저장 경로 지정 필요).

---

## 💡 Obsidian Basic Tips
*(from `ObsidianTips.md`)*

### Essential Shortcuts
- **만들기/이동**: `Cmd + N` (새 노트), `Cmd + O` (빠른 이동)
- **명령 팔레트**: `Cmd + P` (모든 기능 실행)
- **사이드바**: `Cmd + Shift + L` (좌측 토글), `Cmd + Shift + R` (우측 토글)

### Formatting
- **링크**: `[[노트제목]]` (내부 링크), `[이름](URL)` (외부 링크)
- **이미지**: `![[파일이름.png]]` (바로 보기)
- **체크박스**: `- [ ] 할 일`

### Useful Plugins (Recommended)
- **Calendar**: 우측 사이드바에 달력 표시 (저널링 필수).
- **Recent Files**: 최근 열어본 파일 목록.
- **Dataview**: 데이터를 쿼리하여 동적 리스트 생성.

---

## 🧹 Maintenance & Cleanup

### System Folders
- **`.agent/references/`**: 중요한 시스템 참조 파일(`SKILL.md`, `Templates`)이 이곳으로 이동되었습니다. 워크플로우에서 자동으로 참조합니다.
- **`.agent/`**: AI 에이전트 설정 폴더입니다.

### Legacy Cleanup
- `ObsidianTips.md`, `references/` 폴더는 모두 정리(병합/이동) 되었습니다.



---

## 🤖 Agent Workflow Usage

안티그래비티 에이전트를 통해 볼트 관리를 자동화할 수 있습니다. 채팅창에 아래 명령어를 입력하세요.

### 1. Project Kickoff (`/project_kickoff`)
- **언제?**: 새로운 프로젝트를 시작할 때.
- **기능**:
  - 프로젝트 이름 중복 검사.
  - 유형(Study/Dev)에 따른 맞춤형 폴더/파일 생성.
  - `Overview`, `Plan`, `Git` 초기 설정 자동화.

### 2. Weekly Review (`/weekly_review`)
- **언제?**: 매주 일요일 저녁.
- **기능**:
  - `00_Inbox` 잔여 파일 분석 및 정리 제안.
  - 지난 주 활동 로그(수정 파일) 요약.
  - 완료된 할 일(`- [x]`)과 배운 점(`TIL`)을 주간 노트로 자동 집계.

### 3. Archive Project (`/archive_project`)
- **언제?**: 프로젝트가 완료되었을 때.
- **기능**:
  - 외부 의존성(Backlink) 검사로 깨진 링크 방지.
  - 정크 파일(`.DS_Store` 등) 청소.
  - `90_Archives/YYYY/` 로 안전하게 이동 및 기록(`_archive_meta.md`).

### 4. Concept Distiller (`/concept_distiller`)
- **언제?**: 프로젝트 중 중요한 개념을 배웠거나, 스크랩을 정리할 때.
- **기능**:
  - 문맥을 분석해 핵심 키워드(Concept) 추출.
  - `20_Learning` 내 중복 검사.
  - 원본 파일의 단어를 링크(`[[Concept]]`)로 자동 치환하여 연결성 강화.

### 5. MOC Builder (`/moc_builder`)
- **언제?**: 특정 주제의 노트가 5개 이상 쌓여서 한눈에 보고 싶을 때.
- **기능**:
  - 폴더나 태그를 스캔하여 노트 수집.
  - 하위 주제별 자동 그룹화(Clustering).
  - `{Topic}_MOC.md` 생성 및 하위 노트와 연결.

