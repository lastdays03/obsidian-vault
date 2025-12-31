# Templater

**Source**: Data Source: [[08_Automation]]
**Tags**: #concept #plugin #automation

## Definition
Templater는 옵시디언의 **템플릿 엔진** 플러그인으로, JavaScript를 사용하여 날짜, 파일명, 커서 위치 등 동적인 데이터를 템플릿에 삽입할 수 있게 해줍니다.

## Usage
- **Syntax**: `<% ... %>` 안에 코드를 작성합니다.
- **Functions**:
    - `tp.file.creation_date()`: 생성일
    - `tp.file.title`: 파일 제목
    - `tp.file.cursor()`: 생성 후 커서 위치

## Example
```javascript
Title: <% tp.file.title %>
Date: <% tp.date.now("YYYY-MM-DD") %>
```
