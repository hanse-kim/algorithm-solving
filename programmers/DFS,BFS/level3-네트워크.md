# 깊이/너비 우선 탐색 - 타겟 넘버 [🔗](https://programmers.co.kr/learn/courses/30/lessons/43162)

## 풀이

1. 주어진 `computers`를 익숙한 그래프 형태로 바꿔준다.
2. 모든 노드를 하나씩 방문하며 `visited`에 기록한다. 방문하지 않은 노드에 방문할 때마다 네트워크 개수를 +1한다.
3. 방문은 같은 네트워크의 모든 노드를 방문할 때까지 재귀적으로 이뤄지며 항상 `visited`를 갱신한다.

```python
import collections

def visit(node, graph, visited):
    visited.add(node)
    next_nodes = graph[node]
    for next_node in next_nodes:
        if next_node not in visited:
            visit(next_node, graph, visited)

def solution(n, computers):
    graph = collections.defaultdict(list)
    for i in range(n - 1):
        for j in range(i + 1, n):
            if i != j and computers[i][j] == 1:
                graph[i].append(j)
                graph[j].append(i)
    
    network_count = 0
    visited = set()
    for node in range(n):
        if node not in visited:
            visit(node, graph, visited)
            network_count += 1
    
    return network_count
```

