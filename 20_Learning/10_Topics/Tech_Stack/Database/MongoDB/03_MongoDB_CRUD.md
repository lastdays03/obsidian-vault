---
tags: [knowledge/topic, database/mongodb, crud]
Up: [[MongoDB_MOC]]
Source: https://github.com/lastdays03/study_mongodb/blob/main/study/03_crud.md
---

# 03. MongoDB CRUD (기본 연산)

## 📖 정의 (Definition)
MongoDB의 핵심 데이터 조작 기능인 **C**reate(생성), **R**ead(조회), **U**pdate(수정), **D**elete(삭제) 명령어입니다.

## 💡 사용법 (Usage)

### 1. Create (삽입)
```javascript
db.users.insertOne({ name: "Kim", age: 30 })
```

### 2. Read (조회)
```javascript
// 조건 검색 ($gte: 이상)
db.users.find({ age: { $gte: 30 } })
```

### 3. Update (수정)
**`$set` 연산자**를 사용해야 특정 필드만 수정됩니다.
```javascript
db.users.updateOne(
  { name: "Kim" },
  { $set: { age: 31 } }
)
```

### 4. Delete (삭제)
```javascript
db.users.deleteOne({ name: "Kim" })
```

## 💡 Key Insights
- **연산자 활용**: `$gt`, `$in`, `$set` 등 MongoDB 고유의 연산자 문법(`{ operator: value }`)에 익숙해져야 합니다.
- **부분 수정 주의**: Update 시 `$set` 없이 객체를 넘기면 문서 전체가 덮어씌워질 수 있으므로 주의해야 합니다.
