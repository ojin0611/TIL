# DFS



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

   