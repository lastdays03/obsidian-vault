Source: [[Obsidian_Documentation_Mastery_Overview]]
# 07. Task Management (할 일 관리)

**Goal**: 옵시디언을 강력한 할 일 관리 시스템으로 변신시키기.

## 1. Tasks Plugin
옵시디언의 체크박스(`- [ ]`)에 날짜, 우선순위, 반복 기능을 더해줍니다.
- **문법**: `- [ ] 할 일 #태그 📅 2024-01-01 ⏫`
- **단축키**: `Cmd + P` -> `Tasks: Create or edit task` (커서를 체크박스에 두고 실행).
- **실습**:
    - [ ] 이 줄에 커서를 두고 Tasks 명령을 실행하여 오늘 마감(`📅 Today`) 설정을 해보세요. 🆔 wr758o

## 2. Kanban Plugin
할 일을 보드 형태로 시각화하여 관리합니다.
- **생성 방법**:
    1. 새 파일 생성 -> `Project_Board.md`
    2. `Cmd + P` -> `Kanban: Create new board` 선택.
- **실습**: 'To Do', 'Doing', 'Done' 컬럼을 만들고 위에서 만든 Tasks를 이곳으로 드래그해 보세요.

### 시각화: Kanban 워크플로우
```mermaid
graph LR
    Inbox((Inbox)) -->|분류| Todo[📝 To Do]
    Todo -->|시작| Doing[🔥 Doing]
    Doing -->|완료| Done[✅ Done]
    
    style Inbox fill:#eee,stroke:#333
    style Doing fill:#f96,stroke:#333
    style Done fill:#9f9,stroke:#333
```

---
**Next**: [[08_Automation]] 로 넘어가세요.
