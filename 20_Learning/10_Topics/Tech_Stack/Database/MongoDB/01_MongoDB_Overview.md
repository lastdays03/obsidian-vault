---
tags: [knowledge/topic, database/nosql]
Up: [[MongoDB_MOC]]
Source: https://github.com/lastdays03/study_mongodb/blob/main/study/01_introduction.md
---

# 01. MongoDB Overview (NoSQL 이해)

## 📖 정의 (Definition)
**NoSQL(Not Only SQL)**은 전통적인 RDBMS의 한계를 극복하기 위해 등장한 데이터베이스의 총칭입니다. **MongoDB**는 그 중 가장 대표적인 **Document-Oriented(문서 지향)** 데이터베이스로, 데이터를 JSON과 유사한 BSON 형태로 저장합니다.

## 💡 특징 (Characteristics)
1.  **Schemaless**: 고정된 테이블 스키마 없이 자유롭게 필드를 추가할 수 있습니다.
2.  **Scale-Out**: 수평적 확장이 용이하여 대용량 분산 처리에 적합합니다.
3.  **JSON Style**: 개발자 친화적인 데이터 포맷을 사용합니다.

### RDBMS vs MongoDB 용어 비교

| RDBMS | MongoDB | 설명 |
| :--- | :--- | :--- |
| Table | **Collection** | 문서들의 집합 |
| Row | **Document** | 하나의 레코드 (JSON 객체) |
| Column | **Field** | 문서 내의 키(Key) |
| Primary Key | **_id** | 고유 식별자 |

## 💡 예시 (Example)
MongoDB의 데이터 구조(Document) 예시입니다.

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "name": "홍길동",
  "skills": ["Python", "MongoDB"],
  "address": {
    "city": "Seoul"
  }
}
```

## 💡 Key Insights
- **유연성**: 배열이나 중첩 객체를 하나의 문서에 저장할 수 있어, 복잡한 조인(Join) 없이도 데이터를 표현할 수 있습니다.
- **개발 생산성**: 애플리케이션의 객체 구조를 그대로 DB에 저장하므로 변환 과정이 단순합니다.
