---
tags: [knowledge/topic, framework/flask]
Up: [[Flask_MOC]]
---

# 4. Database Integration (데이터베이스 통합)

Flask는 특정 데이터베이스를 강제하지 않지만, 파이썬 생태계에서 가장 널리 쓰이는 `SQLAlchemy` **ORM (Object Relational Mapping)**은 SQL 대신 파이썬 객체로 데이터베이스를 다루는 기술입니다. Flask에서는 주로 SQLAlchemy를 사용합니다. 또한, 데이터베이스 스키마 변경 사항을 관리하기 위해 **Flask-Migrate** (Alembic 기반)를 함께 사용합니다.

---

## 1. 주요 개념 및 라이브러리

-   **SQLAlchemy**: 파이썬의 강력한 ORM 툴킷입니다. SQL을 직접 작성하지 않고 파이썬 객체로 데이터베이스를 다룰 수 있게 해줍니다.
-   **Flask-SQLAlchemy**: Flask 앱 컨텍스트 내에서 SQLAlchemy를 쉽게 사용할 수 있도록 설정을 간소화하고 추가 기능을 제공하는 확장 플러그인입니다.
-   **Flask-Migrate**: 모델이 변경될 때 테이블을 drop/create 하지 않고, 변경 사항(Migration)을 감지하여 DB 스키마를 업데이트해주는 도구입니다.

---

## 2. 설치

```bash
pip install Flask-SQLAlchemy Flask-Migrate
```

### 데이터베이스별 드라이버(Driver) 설치 요약

SQLAlchemy와 데이터베이스를 연결하려면 각 DB에 맞는 파이썬 드라이버(DB API)를 설치해야 합니다.

| 데이터베이스 | 드라이버 | 설치 커맨드 | 연결 URI 예시 |
| :-- | :-- | :-- | :-- |
| **PostgreSQL** | psycopg2 | `pip install psycopg2-binary` | `postgresql://user:pass@host/db` |
| **MySQL / MariaDB** | mysqlclient | `pip install mysqlclient` | `mysql://user:pass@host/db` |
| **MySQL / MariaDB (Pure)** | pymysql | `pip install pymysql` | `mysql+pymysql://user:pass@host/db` |
| **Oracle** | cx_Oracle | `pip install cx_Oracle` | `oracle://user:pass@host/sid` |
| **SQLite** | (Built-in) | (설치 불필요) | `sqlite:///app.db` |

---

## 3. 기본 설정 및 초기화

`app.py` 또는 `__init__.py`에서의 설정 방법입니다.

```python
# app.py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

app = Flask(__name__)

# config 설정
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db' # DB 파일 경로
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False # 불필요한 이벤트 리스너 비활성화 (메모리 절약)

# 객체 생성
db = SQLAlchemy(app)
migrate = Migrate(app, db)
```

---

## 4. 모델(Model) 정의

데이터베이스 테이블을 파이썬 클래스로 정의합니다. `db.Model`을 상속받습니다.

### 4.1 기본 모델

```python
class User(db.Model):
    __tablename__ = 'users' # 테이블 이름 명시 (생략 시 클래스명 소문자)
    
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    
    # 관계 설정 (One-to-Many: User -> Posts)
    # lazy='dynamic'은 query 객체를 반환하여 추가 필터링 가능하게 함
    posts = db.relationship('Post', backref='author', lazy=True)

    def __repr__(self):
        return f'<User {self.username}>'
```

### 4.2 관계(Relationship) 정의

**1:N (One-to-Many)**: 한 명의 유저가 여러 글을 작성

```python
class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(200), nullable=False)
    content = db.Column(db.Text, nullable=False)
    created_at = db.Column(db.DateTime(), default=db.func.now())
    
    # Foreign Key 설정 (users 테이블의 id 컬럼 참조)
    user_id = db.Column(db.Integer, db.ForeignKey('users.id'), nullable=False)

    def __repr__(self):
        return f'<Post {self.title}>'
```

**N:M (Many-to-Many)**: 글과 태그의 관계 (다대다)
다대다 관계를 위해서는 별도의 **연결 테이블(Association Table)**이 필요합니다.

```python
# 연결 테이블
post_tags = db.Table('post_tags',
    db.Column('post_id', db.Integer, db.ForeignKey('post.id'), primary_key=True),
    db.Column('tag_id', db.Integer, db.ForeignKey('tag.id'), primary_key=True)
)

class Tag(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), nullable=False)

    # Many-to-Many 관계 설정
    posts = db.relationship('Post', secondary=post_tags, lazy='subquery',
                            backref=db.backref('tags', lazy=True))
```

---

## 5. Flask-Migrate 사용법 (마이그레이션)

모델을 정의하거나 수정했다면, 이를 실제 DB에 반영해야 합니다.

1.  **초기화 (최초 1회)**
    ```bash
    flask db init
    # migrations 폴더가 생성됩니다.
    ```

2.  **마이그레이션 파일 생성 (모델 변경 시마다)**
    ```bash
    flask db migrate -m "Add User and Post tables"
    # versions 폴더 안에 변경 사항이 기록된 파이썬 스크립트가 생성됩니다.
    ```

3.  **DB 업그레이드 (적용)**
    ```bash
    flask db upgrade
    # 실제 DB에 테이블이 생성되거나 변경됩니다.
    ```

**참고**: 잘못된 마이그레이션을 취소하고 싶다면 `flask db downgrade`를 사용할 수 있습니다.

---

## 6. CRUD 데이터 조작 (Querying)

Flask Shell(`flask shell`)이나 라우트 함수 내부에서 실행합니다.

### 6.1 Create (생성)

```python
user = User(username='gildong', email='gildong@example.com')
db.session.add(user)
db.session.commit() # 커밋을 해야 DB에 반영됨
```

### 6.2 Read (조회)

```python
# 전체 조회
users = User.query.all()

# 조건 조회 (Primary Key)
user = db.session.get(User, 1) # Flask-SQLAlchemy 3.0+ 권장

# 조건 조회 (Filter)
user = User.query.filter_by(username='gildong').first()
active_users = User.query.filter(User.active == True).all()

# 정렬 및 제한
recent_posts = Post.query.order_by(Post.created_at.desc()).limit(5).all()

# 페이지네이션 (Pagination)
page = Post.query.paginate(page=1, per_page=10)
print(page.items) # 현재 페이지 데이터
print(page.total) # 전체 데이터 수
```

### 6.3 Update (수정)

```python
user = User.query.filter_by(username='gildong').first()
user.username = 'new_gildong'
db.session.commit() # 객체의 속성을 바꾸고 커밋하면 Update 쿼리 실행
```

### 6.4 Delete (삭제)

```python
user = User.query.filter_by(username='new_gildong').first()
db.session.delete(user)
db.session.commit()
```

---

## 7. 팁 및 베스트 프랙티스

-   **Context 오류**: `RuntimeError: Working outside of application context.` 오류가 발생하면 `with app.app_context():` 블록 안에서 코드를 실행해야 합니다.
-   **쿼리 최적화**: 관계 데이터가 많을 경우 `lazy` 로딩 옵션을 적절히 조절하거나 `joinedload` 등을 사용하여 N+1 문제를 방지해야 합니다.
-   **구조 분리**: 모델이 많아지면 `models.py` 하나에 두지 말고 패키지(`models/`)로 분리하거나 기능별로 나누는 것이 좋습니다.

---
## 💡 Key Insights
- **Why ORM?**: SQL을 직접 쓰지 않고 객체(Class)로 DB를 다루는 **SQLAlchemy**는 생산성을 높이고 DB 종류(SQLite -> PostgreSQL 등) 변경을 자유롭게 합니다.
- **Migration is Must**: 개발 중 스키마 변경은 필연적입니다. 초기부터 `Flask-Migrate`를 도입하여 DB 버전 관리를 시작하세요.
- **N+1 Problem**: ORM의 편리함 뒤에 숨은 성능 저하(Lazy Loading)를 조심해야 합니다. 쿼리가 너무 많이 나간다면 로딩 전략을 점검하세요.
