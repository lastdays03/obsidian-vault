---
tags: [knowledge/topic, tool/obsidian]
Up: [[Obsidian_MOC]]
---

Source: [[Obsidian_Documentation_Mastery_Overview]]
# 09. [[Dataview]] Dashboards (데이터뷰)

**Goal**: 내 볼트의 데이터를 쿼리하여 동적인 대시보드 만들기.

## 1. 기본 문법 (DQL)
SQL과 비슷하지만 더 쉽습니다. `` ```dataview `` 블록 안에 작성합니다.
- **List**: 단순 목록
- **Table**: 표 형태 (컬럼 지정 가능)
- **Task**: 할 일 목록

## 2. 실습: 프로젝트 현황판
아래 코드 블록을 복사해서 이 노트에 붙여넣어 보세요. (`10_Projects` 폴더의 파일들을 보여줍니다.)

```dataview
TABLE file.ctime as Created, file.mtime as Modified
FROM "10_Projects"
SORT file.mtime desc
```

## 3. 실습: 이번 주 완료한 일
```dataview
TASK
FROM ""
WHERE completed AND file.mtime >= date(today) - dur(7 days)
```

---
**Next**: [[10_Excalidraw_Visualization]] 로 넘어가세요.

## 💡 Key Insights
*   **노트의 DB화**: Dataview는 정적인 노트 보관소를 동적인 데이터베이스로 변환시켜, 내 지식 베이스에게 직접 '질문(Query)'을 던질 수 있게 해줍니다.
*   **메타데이터의 힘**: 노트 본문(Content)만큼이나 메타데이터(Frontmatter) 관리가 중요한 이유가 바로 Dataview를 통해 재가공할 수 있기 때문입니다.
