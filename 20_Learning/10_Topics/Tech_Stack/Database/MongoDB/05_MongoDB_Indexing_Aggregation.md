---
tags: [knowledge/topic, database/mongodb, optimization]
Up: [[MongoDB_MOC]]
Source: https://github.com/lastdays03/study_mongodb/blob/main/study/05_indexing_and_aggregation.md
---

# 05. Indexing & Aggregation (고급 기능)

## 📖 인덱싱 (Indexing)
**인덱스**는 검색 속도를 획기적으로 높여줍니다. 인덱스가 없으면 모든 문서를 스캔(Collection Scan)해야 합니다.

```javascript
// name 필드 오름차순 인덱스 생성
db.users.createIndex({ name: 1 })
```

## 📖 집계 파이프라인 (Aggregation Pipeline)
데이터를 여러 단계(Stage)로 통과시키며 필터링, 그룹화, 변환하는 프레임워크입니다.

### 주요 Stage
- `$match`: 필터링 (WHERE)
- `$group`: 그룹화 (GROUP BY)
- `$sort`: 정렬 (ORDER BY)

### 예시
```javascript
db.users.aggregate([
  { $match: { age: { $gte: 20 } } },     // 20대 이상
  { $group: { _id: "$age", count: { $sum: 1 } } } // 나이별 인원 수
])
```

## 💡 Key Insights
- **Pipeline Thinking**: 데이터를 수도관(Pipeline)에 흘려보내며 순차적으로 가공한다고 상상하면 복잡한 쿼리도 설계하기 쉽습니다.
- **Performance**: 대용량 데이터 처리 시 적절한 인덱스와 Aggregation 활용은 성능 최적화의 핵심입니다.
