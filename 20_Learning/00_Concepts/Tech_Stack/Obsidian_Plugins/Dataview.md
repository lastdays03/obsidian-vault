# Dataview

**Source**: Data Source: [[09_Dataview_Dashboards]]
**Tags**: #concept #plugin #query

## Definition
Dataview는 옵시디언 볼트 내의 데이터를 **SQL과 유사한 쿼리 언어(DQL)**로 조회하여 동적인 리스트나 표를 만들어주는 플러그인입니다.

## Usage
`` ```dataview `` 코드 블록 안에 작성합니다.
- **TABLE**: 표 형식 출력.
- **LIST**: 리스트 형식 출력.
- **TASK**: 할 일 목록 출력.

## Example
```dataview
TABLE file.ctime as Created
FROM "10_Projects"
SORT file.ctime desc
```
