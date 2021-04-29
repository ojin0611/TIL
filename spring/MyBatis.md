# MyBatis

MyBatis는 Java Object와 SQL문 사이의 자동 Mapping기능을 지원하는 ORM Framework다.

[MyBatis Documentation (KO)](https://mybatis.org/mybatis-3/ko/index.html)

MyBatis는 익숙한 SQL을 그대로 이용하면서 JDBC 코드 작성의 불편함을 제거해주고, 도메인 객체나 VO 객체를 중심으로 개발할 수 있게 해준다.

## 특징

- 쉬운 접근성과 코드의 간결함
  - XML 형태로 서술된 JDBC 코드. 복잡한 JDBC 코드를 걷어내어 깔끔한 소스코드 유지
  - 가장 간단한 persistence framework (데이터베이스와의 연동되는 시스템을 빠르게 개발하고 안정적인 구동을 보장해주는 프레임워크)

- SQL문과 프로그래밍 코드의 분리
  - SQL 작성과 관리 또는 검토를 DBA같은 (개발자가 아닌) 다른 사람에게 맡길 수 있다.
- 다양한 프로그래밍 언어로 구현할 수 있다.