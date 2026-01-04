---
tags: [knowledge/topic, tool/obsidian]
Up: [[Obsidian_MOC]]
---

Source: [[Obsidian_MOC]]
# 08. Automation (자동화)

**Goal**: 반복적인 작업을 자동화하여 시간을 절약하기.

## 1. [[Templater]] (템플릿 엔진)
단순한 텍스트 복사/붙여넣기를 넘어, 날짜나 커서 위치 등을 동적으로 처리합니다.
- **설정**: `Settings` -> `Templater` -> `Template folder location`을 `99_System/Templates`로 지정하세요.
- **실습**:
    1. `99_System/Templates` 폴더에 `Daily_Review.md` 파일을 만듭니다.
    2. 내용에 `<% tp.file.creation_date() %>`를 적습니다.
    3. 새 노트에서 `Cmd + P` -> `Templater: Insert template`를 실행하여 적용해 보세요.

## 2. Linter (포맷팅 자동화)
문서의 스타일을 일관되게 유지해 줍니다.
- **설정**: `General` -> `Lint on save`를 켜두면 저장할 때마다 자동으로 정리됩니다.
- **실습**: 리스트의 들여쓰기를 엉망으로 하고 저장을 눌러보세요. Linter가 정리해 줍니다.

---
**Next**: [[09_Dataview_Dashboards]] 로 넘어가세요.

## 💡 Key Insights
*   **일관성의 보증**: Templater나 Linter 같은 자동화 도구는 게으름을 위한 것이 아니라, 인간의 기억력으로는 유지하기 힘든 '일관성(Consistency)'을 기계적으로 보장하기 위함입니다.
*   **시스템적 사고**: 반복 작업을 자동화하는 과정에서, "이 작업의 패턴은 무엇인가?"를 고민하게 되어 업무를 시스템적으로 바라보는 눈이 생깁니다.
