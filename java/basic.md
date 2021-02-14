

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



## 파일 내에 클래스 여러개 정의

 



## 클래스 비교

### compareTo

comparable interface를 implements해준다.



>  이외에도 2가지 더 있다.



