[TOC]



# 스택

후입선출구조(LIFO)

선형구조

## Stack API

`java.util.Stack`

```java
import java.util.Stack;

Stack<Integer> stack = new Stack<>(); //int형 스택 선언
```



주요 메소드

`push()` ,

`pop()` : 맨위의 객체 꺼내어 반환. 비어있으면 Exception.

`peek()` : 맨 위의 객체 반환. 꺼내지 않음. 비어있으면 Error

`isEmpty()`, `size()`

`search(Object o)` : 객체 o를 찾아서 위치 반환. 못찾으면 -1

# 큐

선입선출구조 (FIFO)

큐는 맨 처음 front, rear가 같은 곳(빈 곳)을 바라보고 있다. 원소를 추가하면 뒤에 추가되고, rear는 가장 최근에 삽입된 원소를 가리킨다. 

원소를 반환하면 맨 처음 넣은 원소가 빠져나가고, front가 그 다음에 들어온 원소를 가리킨다.

## Queue API

`java.util.Queue`

LinkedList 클래스를 Queue 인터페이스의 구현체로 많이 사용한다.

```java
import java.util.LinkedList; 
import java.util.Queue; 

Queue<Integer> queue = new LinkedList<>();
```



주요 메소드

`offer() ` 삽입.

`poll()`  꺼내서 반환

`peek()` 삭제없이 요소 읽기. 비어있으면 null. `element()` 비어있으면 Error

`isEmpty()`, `size()`

`search(Object o)` : 객체 o를 찾아서 위치 반환. 못찾으면 -1



# 우선순위 큐

선입선출 순서가 아니라, 우선순위가 높은 순서대로 먼저 나가게 된다.



## Priority Queue API

`java.util.PriorityQueue`

Heap 자료구조다. 힙은 완전 이진트리로 구현된 자료구조인데, 최소 힙이나 최대 힙으로 자주 사용된다. 키값이 가장 작은/큰 노드가 항상 루트에 위치하는 자료구조로, 힙의 키를 우선순위로 활용하면 우선순위 큐를 구현할 수 있다.



```java
import java.util.PriorityQueue; //import

//int형 priorityQueue 선언 (우선순위가 낮은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();

//int형 priorityQueue 선언 (우선순위가 높은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(Collections.reverseOrder());
```



### 주요 메소드

추가 : `add()`, `offer()`

삭제 : `poll()`, `remove()`, `clear()`

참조 : `peek()`



# 덱

자바에서의 덱(Deque)은 인터페이스로 구현돼있다. 이를 구현한 클래스로는 ArrayDeque, LinkedList 등이 있다.

사용되는 메소드는 다음과 같다.

![image-20210311142649782](images/image-20210311142649782.png) 

![image-20210311142949766](images/image-20210311142949766.png) 

![image-20210311143004207](images/image-20210311143004207.png) 

