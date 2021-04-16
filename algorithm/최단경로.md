# 최단 경로

간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치의 합이 최소인 경로



# 다익스트라 (Dijkstra)

하나의 시작 정점에서 끝 정점까지의 최단 경로를 구하는 알고리즘으로, 음의 가중치를 허용하지 않는다.

```
Dijkstra(start, adjMatrix, distance)
	selected[start] = true;
	for all v:
		distance[v] = adjMatrix[start][v]
	
	while (not all selected)
		w // distance[w]가 최소인 정점, w is not selected
        selected[w] = true
        for w와 인접한 미방문 정점 v:
        	distance[v] = min(distance[v], distance[w]+adjMatrix[w][v])
```



계산 속도를 높이기 위해 인접 행렬 대신 인접 리스트를 사용하거나 우선순위 큐를 사용하기도 한다.

```java
class Solution {
	static int N, E, INF;
	static List<Point> list[];
	static int[] dist;
	static boolean[] check;
    
    public static void main(String[] args){
        N = 3; E = 1; INF = 987654321;
        
        // 인접 리스트 형태로 그래프 저장
        list = new ArrayList[N + 1];
        for(int i = 0; i <= N; i++)
            list[i] = new ArrayList<>();
        
        // 시작 노드에서 다른 노드까지 거리
        dist = new int[N + 1];
        
        // dist를 갱신할 때 해당 노드를 고려했는가
        check = new boolean[N + 1];

        for(int i = 0 ; i < E; i++){
            int start = 1;
            int end = 2;
            int weight = 3;
            list[start].add(new Point(end, weight));
            list[end].add(new Point(start, weight));
        }
        
        System.out.println(dijkstra(1, N));
    }
    
    private static int dijkstra(int start, int end) {
		Arrays.fill(dist, INF);
		Arrays.fill(check, false);
		
		// PriorityQueue에 정점들을 넣는다. 시작점에서의 거리가 짧은것부터 뽑는다.
		PriorityQueue<Point> pq = new PriorityQueue<>();
		pq.add(new Point(start, 0));
		dist[start] = 0; // 1번 Node부터 존재. 없는 점
		
		// 시작점에서 거리가 가까운 정점부터 방문하면서 갱신
		while(!pq.isEmpty()) {
			Point point = pq.poll();
			int node = point.end;
			int weight=point.weight;
			
            // 현재 노드를 고려했음
			check[node] = true;
			
			// 현재 노드와 인접한 노드들을 방문한 경로와 기존 경로 비교
			for (int i = 0; i < list[node].size(); i++) {
				int nextNode  = list[node].get(i).end;
				int nextWeight= list[node].get(i).weight;
				
                // 이미 고려했던 node라면 continue
				if(check[nextNode]) continue;
				
				// 최단경로가 갱신됐다면
				if(weight+nextWeight < dist[nextNode]) {
					dist[nextNode] = weight + nextWeight;
					pq.add(new Point(nextNode, dist[nextNode]));
				}
			}
		}
		
		return dist[end];
	}
    
	static class Point implements Comparable<Point>{
		int end, weight;

		public Point(int end, int weight) {
			this.end = end;
			this.weight = weight;
		}

		@Override
		public int compareTo(Point o) {
			return this.weight - o.weight;
		}
	}
    
}
```





# 벨만 포드 (Bellman-Ford)

하나의 시작 정점에서 끝 정점까지의 최단 경로를 구하는 알고리즘으로, 음의 가중치를 허용한다.



# 플로이드 와샬 (Floyd-Warshall)

모든 정점들에 대한 최단 경로를 구하는 알고리즘이다.



```java
// 점화식 : dp_ij_n = min(dp_ik_n-1 + dp_kj_n-1, dp_ij_n-1)

// dp 초기화
dp[i][j] = distance; // i -> j
if(i!=j && dp[i][j] == 0) dp[i][j] = INF;

// k지점을 지나간 경로까지 갱신
for (int k = 0; k < N; k++) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            dp[i][j] = Math.min(dp[i][k] + dp[k][j], dp[i][j]);
        }
    }
}

```

