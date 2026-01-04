---
tags: [knowledge/concept, plugin/templater]
Up: [[Obsidian_MOC]]
---

# 개념 (Concept): Templater

**출처**: [[08_Automation]]

---

## 📖 정의 (Definition)
**Templater**는 옵시디언의 **템플릿 엔진** 플러그인으로, JavaScript를 사용하여 날짜, 파일명, 커서 위치 등 동적인 데이터를 템플릿에 삽입할 수 있게 해줍니다.

---

## 💡 예시 (Example)
`<% ... %>` 구문을 사용하여 동적인 값을 삽입합니다.

```javascript
Title: <% tp.file.title %>
Date: <% tp.date.now("YYYY-MM-DD") %>
Cursor: <% tp.file.cursor() %>
```

## 💡 Key Insights
- **Workflow Automation**: 단순한 텍스트 복사를 넘어, 파일 생성 시점에 자동으로 폴더를 이동하거나, 태그를 붙이는 등 '행동'을 자동화할 수 있습니다.
- **System Integration**: `tp.user` 모듈을 통해 외부 쉘 스크립트나 파이썬 코드를 실행할 수도 있어 확장성이 무한합니다.
