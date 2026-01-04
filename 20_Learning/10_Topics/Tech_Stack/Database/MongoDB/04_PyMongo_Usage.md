---
tags: [knowledge/topic, python/library, database/mongodb]
Up: [[MongoDB_MOC]]
Source: https://github.com/lastdays03/study_mongodb/blob/main/study/04_python_driver.md
---

# 04. PyMongo Usage (파이썬 연동)

## 📖 정의 (Definition)
**PyMongo**는 Python에서 MongoDB를 다루기 위한 공식 드라이버 라이브러리입니다.

## 💡 설치 및 연결
```bash
pip install pymongo
```

```python
from pymongo import MongoClient

# 연결 및 DB/Collection 선택
client = MongoClient('mongodb://localhost:27017/')
db = client['example_db']
collection = db['users']
```

## 💡 CRUD 예제
Python의 딕셔너리(`dict`)를 그대로 저장하고 조회할 수 있습니다.

```python
# Create
collection.insert_one({"name": "Jongman", "age": 30})

# Read
for doc in collection.find({"age": {"$gte": 20}}):
    print(doc)

# Update
collection.update_one({"name": "Jongman"}, {"$set": {"age": 31}})
```

## 💡 Key Insights
- **Snake Case**: MongoDB Shell(`camelCase`)과 달리 Python에서는 `insert_one`, `find_one`과 같이 **snake_case** 메소드명을 사용합니다.
- **Pythonic**: 파이썬의 `dict` 자료형과 MongoDB의 Document가 1:1로 매핑되어 매우 직관적인 코딩이 가능합니다.
