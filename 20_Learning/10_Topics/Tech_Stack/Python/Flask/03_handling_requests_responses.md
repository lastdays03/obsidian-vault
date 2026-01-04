---
tags: [knowledge/topic, framework/flask]
Up: [[Flask_MOC]]
---

# 3. Handling Requests and Responses (요청과 응답 처리)

웹 애플리케이션은 클라이언트의 요청(Request)을 받아 처리하고 응답(Response)을 보내는 과정의 연속입니다. Flask에서 이를 어떻게 다루는지 알아봅니다.

## 1. Request 객체

Flask의 `request` 객체는 글로벌 객체처럼 보이지만, 실제로는 각 요청에 맞는 컨텍스트 로컬 객체입니다.

```python
from flask import request
```

### 데이터 접근 방법
- **URL 쿼리 파라미터 (`?key=value`)**: `request.args`
  ```python
  search_keyword = request.args.get('q', '')
  ```
- **Form 데이터 (POST 요청)**: `request.form`
  ```python
  username = request.form['username']
  ```
- **JSON 데이터**: `request.json` 또는 `request.get_json()`
  ```python
  data = request.get_json()
  ```

## 2. HTTP Methods

기본적으로 라우트는 `GET` 요청만 받습니다. 다른 메서드를 받으려면 `methods` 인자를 추가해야 합니다.

```python
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()
```

## 3. Response 객체

View 함수는 문자열을 반환할 수 있지만, 헤더나 상태 코드를 변경하고 싶을 때는 `make_response`를 사용하거나 튜플을 반환합니다.

### 튜플 반환 (Body, Status, Headers)
```python
@app.route('/tuple')
def return_tuple():
    return 'Bad Request!', 400
    # 또는
    # return 'Hello', 200, {'X-Custom-Header': 'Value'}
```

### JSON 응답
API 서버를 만들 때 유용합니다. `jsonify` 함수를 사용합니다.
```python
from flask import jsonify

@app.route('/api/data')
def get_data():
    return jsonify({'key': 'value', 'list': [1, 2, 3]})
```

## 4. 리다이렉션과 에러

### 리다이렉트 (Redirect)
사용자를 다른 페이지로 이동시킵니다.
```python
from flask import redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))
```

### 에러 처리 (Abort & Error Handler)
`abort`로 에러를 발생시키고, `errorhandler`로 이를 처리합니다.

```python
from flask import abort, render_template

@app.route('/user/<id>')
def get_user(id):
    user = load_user(id)
    if not user:
        abort(404) # 404 Not Found 발생
    return user

@app.errorhandler(404)
def page_not_found(error):
    return render_template('404.html'), 404
```

---
## 💡 Key Insights
- **Request Lifecycle**: `request` 객체는 쓰레드 로컬(Thread Local)하게 동작하여, 동시에 들어오는 요청들을 서로 간섭 없이 독립적으로 처리합니다.
- **REST Compliance**: 적절한 HTTP Method(`GET`, `POST`)와 Status Code(`200`, `404`)를 사용하는 것은 웹 표준을 지키는 API 설계의 기본입니다.
- **Robustness**: `abort(404)`와 `errorhandler`를 적극적으로 활용하여 예외 상황을 우아하게 처리하세요.
