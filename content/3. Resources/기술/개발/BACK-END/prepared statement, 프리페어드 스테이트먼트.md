## 요약
- 미리 컴파일 완료된 쿼리를 사용하는 방법으로 SQL Injection을 막고 SQL문 파악에 어려움을 준다.

## 내용
DBMS에서 동일하거나 비슷한 데이터베이스문을 높은 효율성으로 반복적으로 실행하기 위해 사용되는 기능이다. 일반적으로 쿼리나 업데이트와 같은 SQL문과 함께 사용되는 프리페어드 스테이트먼트는 템플릿의 형태를 취하며, 그 템플릿 안으로 특정한 상수값이 매 실행 때마다 대체 된다. 즉, *이미 쿼리실행계획 분석과 컴파일이 완료되어서 DBMS의 캐시에 준비되어있는 쿼리를 사용한다는 의미*이다.

DBMS 쿼리를 실행하는 절차와 각 부분에서 처리하는 내용은 아래와 같다.
1. Parsing (Query의 문법적, 의미적 오류 체크, 재사용 가능 SQL 확인, 쿼리실행계획 수립 등)
2. Execution (쿼리를 실행한다.)
3. Fetch(실행된 값을 가져오는 절차이므로 Select 만 해당됨. 반환하는 값이 없는 Insert, Update, Delete는 미해당.)

```sql
String sqlstr = "SELECT name, memo FROM TABLE WHERE num = ?" PreparedStatement stmt = conn.preparedStatement(); stmt.setInt(1, num); ResultSet rst = stmt.executeQuery(sqlstr);
```
예시는 위와 같으면 미리 컴파일이 되어 있기 때문에 Statement에 비해서 성능이 좋고 특수 문자를 자동으로 파싱하여 SQL Injection 같은 공격을 막을 수 있다. 또한 '?' 부분에만 변화를 주어 쿼리문을 수행하므로 실행되는 SQL문을 파악하기 어렵다.

- Prepared statement를 사용해야하는 경우는...
	- 사용자 입력값으로 쿼리문을 실행하는 경우
	- 쿼리 반복 수행 작업인 경우

## 참고
- https://iksflow.tistory.com/127