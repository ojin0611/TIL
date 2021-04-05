# MySQL



# Case When

```mysql
select employee_id, last_name,
case when salary < 10000 then '초급'
	 when salary < 20000 then '중급'
	 else '고급'
end as '구분'
from employees
```



# 내장함수

## 숫자

- abs(n) : 절대값
- ceiling(n) : 정수 올림 
- floor(n) : 정수 내림
- round(n, 자릿수) : 반올림

- truncate(n, 자릿수) : 버림

- pow(x,y) or power(x,y) : x**y
- mod(a,b) : a % b
- greatest(a,b,c,...) : 가장 큰 수
- least(a,b,c,...) : 가장 작은 수

```mysql
select ceil(12.2) from dual;
-- dual은 계산을 위한 가상의 테이블 이름
```



## 문자

- ASCII(a) : 아스키코드값
- concat(a,b,c) : 문자열 결합
- insert(a,start,length, new) : a의 start 위치부터 length만큼 new로 대치
- replace(a,b,c) : a의 b를 c로 변경
- instr(a,b) : a에서 b의 위치 리턴
- mid(a, b, count) : a에서 b부터 count개만큼 리턴
- substring(a,b,count) : a중 b부터 count개만큼 리턴
- ltrim(a), rtrim(a), trim(a) : 공백 제거

- lcase(a) or lower(a) : 소문자
- ucase(a) or upper(a) : 대문자
- left(a, count) : 문자열 중 왼쪽에서 count개 추출
- right(a, count) : 문자열 중 오른쪽에서 count개 추출

- reverse(a): 반대로 나열



## 날짜

- now(), sysdate(), current_timestamp() : 현재 날짜와 시간
- curdate(), current_date() : 현재 날짜
- curtime(), current_time() : 현재 시간

- date_add(date, interval) : 날짜에서 기준값 더하기
- date_sub(date, interval) : 날짜에서 기준값 빼기
- year(date) /month(date) : 연/월 리턴
- monthname(date) : 날짜의 월을 영어로 리턴
- dayname(date) : 날짜의 요일을 영어로 리턴

- dayofmonth(date) : 월별 날짜 리턴
- dayofweek(날짜) : 날짜의 주별 일자 리턴 (1~7)
- weekday(날짜) : 날짜의 주별 일자 리턴 (0~6)
- dayofyear(날짜) : 365일중 몇번째일
- week(날짜) : 일년중 몇번째주
- from_days(days) : 0년0월0일부터 days만큼 경과한 날의 날짜
- to_days(date) : 0년0월0일부터 date까지 일자 수
- date_format(date, format) : 날짜를 형식에 맞게 리턴

### 날짜 형식

![image-20210405175920789](images/image-20210405175920789.png) 



## 논리

- if(bool, a, b) : bool이 참이면 a 거짓이면 b 리턴
- ifnull(a,b) : a가 null이면 b로 대치
- nullif(a,b) : a==b 면 null, 그렇지 않으면 a



## 그룹

count(col), sum(col), avg(col), max(col), min(col)



## Transaction

- Start Transaction : commit, rollback이 나올때까지 실행되는 모든 sql
- commit : 모든코드 실행
- rollback : start transaction 실행 전 상태로 되돌림 (savepoint 이용해서 저장)