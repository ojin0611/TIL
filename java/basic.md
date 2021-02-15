[TOC]

# 문장

## 대입 연산자의 리턴값

```java
int a = 0;
System.out.println(a=3);
```

위의 코드를 실행하면 3이 출력된다. 즉, 대입 연산자에는 리턴값이 존재한다.

이를 이용해 LinkedList의 다음 노드를 계속 탐색할 수 있다.



```java
while ((nextNode = currNode.link) != null) { // nextNode = currNode가 가리키는 노드
    if (nextNode == target) { // 다음 노드가 target이라면
        return currNode; // 현재 노드 리턴
    }
    currNode = nextNode;
}

```



# Class

## 하나의 자바 파일 내에 여러 개의 클래스 정의



## Inner Class



# 제어자

## 접근 제어자

public

protected

default

private

## 그 외 제어자

static

final

abstract

synchronized



# Singleton Pattern



# Lambda



# Serializable



# Exception



# 