# BigInteger

BigInteger은 문자열 형태로 이루어져 있어 숫자의 범위가 무한하기에 어떠한 숫자이든지 담을 수 있습니다.



## 선언

```java
BigInteger b1 = new BigInteger("10000"); // 생성자는 parameter로 문자열을 받는다.
BigInteger b2 = BigInteger.ONE; // 1
BigInteger b3 = BigInteger.valueOf(12345); // int -> BigInteger
```



## 계산

```java
b1.add(b2); // 덧셈 +
b1.subtract(b3); // 뺄셈 -
b2.multiply(b1); // 곱셈 *
b3.divide(b2); // 나눗셈 /
b3.remainder(b1); // 나머지 %
```



## 형변환

```java
int intB1 = b1.intValue();
long longB1 = b1.longValue();
float floatB1 = b1.floatValue();
double doubleB1 = b1.doubleValue();
String stringB1 = b1.toString();
```



## 숫자 비교

```java
int compare = b1.compareTo(b2);
// 두 수 비교해서 맞으면 0, 틀리면 -1
```

