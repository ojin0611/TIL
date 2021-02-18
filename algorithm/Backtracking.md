# 백트래킹

상태 공간 트리를 만든다. 당첨 리프 노드를 찾는 것이 핵심이다.

꽝 노드 또는 더 이상의 선택지가 없다면 이전의 선택지로 돌아가서 다른 선택을 한다. 루트까지 돌아갔을 경우 더 이상 선택지가 없다면 찾는 답이 없다.

## 백트래킹 기법

어떤 노드의 유망성을 점검한 후에 유망(promising)하지 않다고 결정되면 그 노드의 부모로 되돌아가(backtracking) 다음 자식 노드로 간다. 즉, 불필요한 경로를 조기에 차단한다 (pruning).



-  유망(promising)하다 : 어떤 노드를 방문했을 때, 그 노드를 포함한 경로가 해답이 될 수 있을 때
- 가지치기(pruning) : 유망하지 않은 노드가 포함되는 경로를 고려하지 않는다.



## 절차

1. 상태 공간 트리의 깊이우선탐색(dfs)을 실시한다.
2. 각 노드가 유망한지 점검한다.
3. 만일 그 노드가 유망하지 않으면, 그 노드의 부모 노드로 돌아가서 다른 노드로의 검색을 계속한다.



```
backtrack(node v)
	if promising(v) == false // 여기가 dfs와 큰 차이점. 가지치기!
		return;
	if there is a solution at v
		write the solution
	else
		for each child u of v
			backtrack(u)
```

