# 그래프

그래프는 정점(Vertex)들의 집합과 이들을 연결하는 간선(Edge)들의 집합으로 구성된 자료구조다. 



## 그래프의 유형

![image-20210316093701610](images/image-20210316093701610.png) 

트리는 사이클이 없는 무향 연결 그래프다.

완전 그래프 : 가능한 모든 간선을 가진 그래프

부분 그래프 : 원래 그래프에서 일부의 정점이나 간선을 제외한 그래프



## 그래프 표현

간선의 정보를 저장하는 방식, 메모리나 성능을 고려해서 결정한다.



### 인접 행렬(Adjacent Matrix)

V x V 크기의 2차원 배열 이용해 간선 정보를 저장한다. 배열의 배열

![image-20210316102858998](images/image-20210316102858998.png) 

### 인접 리스트(Adjacent List)

각 정점마다 다른 정점으로 나가는 간선의 정보를 저장한다. 하나의 정점에 대한 인접 정점들을 각각 노드로 하는 연결 리스트로 저장한다.

정점에 비해 간선이 적을 경우(희소 그래프), 인접 행렬에 비해 효율적으로 사용할 수 있다.

 ![image-20210316102708050](images/image-20210316102708050.png) 





### 간선 리스트(Edge List)

간선(시작 정점, 끝 정점)의 정보를 객체로 표현해 리스트에 저장한다.

![image-20210316104341576](images/image-20210316104341576.png) 



# BFS

너비 우선 탐색



아래 3가지를 꼭 기억하자. 예제는 [백준 2206 벽 부수고 이동하기](https://www.acmicpc.net/source/26882834) 의 자바 코드

1. 내가 필요한 것을 변수로 만들어서 Class로 가지고다니기

   ```java
   class Position {
   	int r, c, cnt;
   	int power;
   
   	Position(int r, int c, int power, int cnt) {
   		this.r = r;
   		this.c = c;
   		this.power = power;
   		this.cnt = cnt;
   	}
   }
   ```

   

2. 방문 배열을 int형으로 생성하고, 무한대로 초기화한다.

   ```java
   // initialize visit 
   visit = new int[N][M];
   for (int i = 0; i < N; i++) {
       for (int j = 0; j < M; j++) {
           visit[i][j] = 1000000;
       }
   }
   ```

   

3. BFS 안에서 돌면서 주변을 탐색하면 같은 위치를 여러번 탐색하게 된다. 이 때 방문 배열의 최소값을 계속 들고다닌다. 들고다니는 값과 방문 배열의 값을 비교하면서 방문 여부를 검사한다.

   ```java
   // 이미 방문했다면
   if (visit[nr][nc]<=power) continue;
   ```

   



# DFS

재귀로 푼다.

