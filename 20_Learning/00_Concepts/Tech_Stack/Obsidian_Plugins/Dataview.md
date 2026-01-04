---
tags: [knowledge/concept, plugin/dataview]
Up: [[Obsidian_MOC]]
---

# 개념 (Concept): Dataview

**출처**: [[09_Dataview_Dashboards]]

---

## 📖 정의 (Definition)
**Dataview**는 옵시디언 볼트 내의 메타데이터(Frontmatter, Inline Field)를 **SQL과 유사한 쿼리 언어(DQL)**로 조회하여 동적인 리스트나 표를 만들어주는 플러그인입니다.

---

## 💡 예시 (Example)
`` ```dataview `` 코드 블록을 사용하여 파일 목록을 자동 생성할 수 있습니다.

```dataview
TABLE file.ctime as Created
FROM "10_Projects"
SORT file.ctime desc
LIMIT 5
```

## 💡 Key Insights
- **동적 노트**: 수동으로 목록을 업데이트할 필요 없이, 데이터뷰 쿼리를 심어두면 항상 최신 상태의 인덱스 페이지를 유지할 수 있습니다.
- **메타데이터 활용**: 텍스트뿐만 아니라 `status`, `due_date` 같은 속성(Property)을 적극적으로 활용하게 만드는 동기가 됩니다.
