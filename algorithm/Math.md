[TOC]



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



# 페르마의 소정리

p가 소수, a가 정수. a가 p의 배수가 아니라면 아래 식을 만족한다.

![a^{p}\equiv a{\pmod  p}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7ff656f721894b9a50a2b1d18538463a6a4ec15f) 

양변을 약분해서 다음과 같이 쓸 수 있다.

![a^{{p-1}}\equiv 1{\pmod  p}\qquad (a\neq 0)](https://wikimedia.org/api/rest_v1/media/math/render/svg/2149302899fcbf99c1b46c536549f7ed7b0a6b2b) 

이를 이용해 nCr을 구할 수 있다. (mod p)

nCr = n! / (r! (n-r)! )  

≡ n! / k (mod p) = n! * k**(p-2)

> 여기서 k는 r! * (n-r)! 을 p로 나눈 나머지다.
>
> r!, (n-r)!을 각각 p로 나눈 나머지 k1, k2를 이용해 n! * (k1)\**(p-2) * (k2)\**p-2 로 표현할 수 있다.



```java
// nCr
private static long comb(int n, int r, long p) {
    if(r==0) return 1L;
    // n! / (r! * (n-r)!)
    // (n! * r!**(p-2)%p * (n-r)!**(p-2)%p ) % p
    long[] fac = new long[n+1];
    fac[0] = 1;
    for (int i = 1; i < fac.length; i++) {
        fac[i] = fac[i-1] * i % p;
    }
    return 
        (fac[n] 
         * power(fac[r], p-2, p) % p
         * power(fac[n-r], p-2, p) % p
        ) %p;
}

// x ** y % p
private static long power(long x, long y, long p) {
    long res = 1L;
    x = x%p;
    while(y>0) {
        // y 2로 나누면서 x에 연산
        if(y%2==1) 
            res = (res * x) % p;
        y = y >> 1;
        x = (x*x)%p;
    }
    return res;
}

```



## 뤼카의 정리

페르마의 소정리로는 아래처럼 n이 매우 큰 문제를 풀 수 없다.

![img](images/212E9536562CAFCD2C) 

이런 경우 Lucas Theorem을 이용한다.



음이 아닌 정수 n,k, 소수 p에 대해 다음과 같이 n과 k를 p진법 전개식으로 나타냈을 때

![img](images/252A7336562CAFD433) 

다음 합동식이 성립하는 것에 대한 정리다.

![img](images/2615233C562CAFEC30) 

이를 이용해 nCr을 구할 수 있다.

```java
long n,r,p; // 할당됐다고 가정

while (n != 0 && r != 0) {
    answer = (answer * nCr(n % p, r % p, p)) % p;
    n /= p;
    r /= p;
}

// Methods //
private static long nCr(long n, long r, long mod) {
    if (n < r)
        return 0;
    long top = getFactorial(n, mod);
    long bottom = getFactorial(r, mod) * getFactorial(n - r, mod) % mod;

    return top * power(bottom, mod - 2, mod) % mod;
}

private static long getFactorial(long n, long mod) {
    long res = 1;
    for (int num = 2; num <= n; num++)
        res = (res * num) % mod;
    return res;
}

private static long power(long num, long p, long MOD) {
    if (p == 0)
        return 1;
    long half = power(num, p / 2, MOD);
    if (p % 2 == 1) {
        return half * half % MOD * num % MOD;
    } else {
        return half * half % MOD;
    }
}
```

