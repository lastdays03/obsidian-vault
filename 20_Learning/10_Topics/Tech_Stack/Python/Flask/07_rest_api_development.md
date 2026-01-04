---
tags: [knowledge/topic, framework/flask]
Up: [[Flask_MOC]]
---

# 07. REST API 개발 (Rest API Development)

Flask는 가볍고 유연하여 RESTful API 서버를 구축하는 데 매우 적합합니다.

## 1. RESTful 아키텍처 기초

- **Resource**: URI로 식별 (예: `/users`, `/users/1`)
- **Method**: 행위 표현 (GET: 조회, POST: 생성, PUT/PATCH: 수정, DELETE: 삭제)
- **Representation**: 데이터 표현 (주로 JSON)

## 2. JSON 응답 (Data Handling)

### Standardized Response (표준 응답 포맷)
`sample_flask`에서는 API의 일관성을 위해 유틸리티 함수(helper)를 사용하여 응답을 표준화합니다.

**`app/utils.py` 예시**:
```python
from flask import jsonify

def api_response(status, message, data=None, error=None):
    """표준화된 API 응답 포맷을 반환합니다."""
    response = {
        "status": status,  # "success" or "error"
        "message": message,
        "data": data,
        "error": error
    }
    return jsonify(response)

# 사용 예시
# return api_response("success", "Data retrieved", data={"count": 5})
```

## 3. 간단한 REST API 구현 예제

```python
tasks = [
    {'id': 1, 'title': '공부하기', 'done': False},
    {'id': 2, 'title': '운동하기', 'done': False}
]

# 조회 (GET)
@app.route('/tasks', methods=['GET'])
def get_tasks():
    return jsonify({'tasks': tasks})

# 생성 (POST)
@app.route('/tasks', methods=['POST'])
def create_task():
    if not request.json or 'title' not in request.json:
        return jsonify({'error': 'Bad Request'}), 400
    
    task = {
        'id': tasks[-1]['id'] + 1,
        'title': request.json['title'],
        'done': False
    }
    tasks.append(task)
    return jsonify({'task': task}), 201
```

## 4. API 확장 라이브러리

API가 복잡해지면 문서화(Swagger/OpenAPI)와 데이터 검증(Validation)이 중요해집니다. 이를 돕는 확장 라이브러리들이 있습니다.

### Flask-RESTX (또는 Flask-RestPlus)
Swagger 문서를 자동으로 생성해주는 강력한 도구입니다.

**설치**:
```bash
pip install flask-restx
```

**예제**:
```python
from flask import Flask
from flask_restx import Api, Resource, fields

app = Flask(__name__)
api = Api(app, version='1.0', title='Todo API',
    description='A simple Todo API')

ns = api.namespace('todos', description='TODO operations')

todo_model = api.model('Todo', {
    'id': fields.Integer(readonly=True, description='The task unique identifier'),
    'task': fields.String(required=True, description='The task details')
})

class TodoDAO(object):
    def __init__(self):
        self.counter = 0
        self.todos = []
    
    def get(self, id):
        for todo in self.todos:
            if todo['id'] == id:
                return todo
        api.abort(404, "Todo {} doesn't exist".format(id))

    def create(self, data):
        todo = data
        todo['id'] = self.counter = self.counter + 1
        self.todos.append(todo)
        return todo

DAO = TodoDAO()

@ns.route('/')
class TodoList(Resource):
    @ns.doc('list_todos')
    @ns.marshal_list_with(todo_model)
    def get(self):
        '''List all tasks'''
        return DAO.todos

    @ns.doc('create_todo')
    @ns.expect(todo_model)
    @ns.marshal_with(todo_model, code=201)
    def post(self):
        '''Create a new task'''
        return DAO.create(api.payload), 201

if __name__ == '__main__':
    app.run(debug=True)
```
API 서버를 실행하고 `http://localhost:5000/`로 접속하면 Swagger UI를 볼 수 있습니다.

### Flask-Smorest
최신 OpenAPI 스펙을 지원하며 `marshmallow` 스키마 검증과 잘 통합됩니다. 최근 많이 사용되는 추세입니다.

---
## 💡 Key Insights
- **Standardization**: RESTful API는 자원(URI)과 행위(Method)를 분리하여, 누구나 직관적으로 이해할 수 있는 인터페이스를 제공합니다.
- **Documentation**: API는 혼자 쓰는 것이 아닙니다. Flask-RESTX 등을 통해 **Swagger** 문서를 자동화하여 프론트엔드 개발자와의 소통 비용을 줄이세요.
- **Validation**: 입력 데이터 검증은 보안의 첫걸음입니다. 라이브러리를 통해 엄격한 스키마 검증을 적용하세요.
