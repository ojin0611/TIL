# 분할 정복

Divide and Conquer



## 설계 전략

- 분할(Divide) : 해결할 문제를 여러 개의 작은 부분으로 나눈다
- 정복(Conquer) : 나눈 작은 문제를 각각 해결한다
- 통합(Combine) : (필요하다면) 해결된 해답을 모은다.



## 예제

### 거듭제곱

```java
public static long exp(long x,long y) {
    if(y==1) return x;

    long r = exp(x,y/2);
    long result = r*r;

    if(y%2==1) result *= x ;
    return result;

}

```

